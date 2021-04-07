---
layout:      post
title:       Vault Terraform Cloud Init
description: Setting up a Terraform Cloud workspace to manage Vault congfiguration
author:      Lucy Davinhart
date:        2021-04-07 19:00:00
category:    tech
tags:        [terraform, terraform cloud, vault]
---

So I've had my own personal Vault for ages...

And a couple of years ago, I spoke at HashiConf[^1] about how we[^2] manage our Vault configuration with Terraform. So it feels like it's about time I get around to doing something similar for my own Vault.

For now, I'm not gonna do anything fancy; I just want bare minimum Vault Terraformability.

I'll be using [Terraform Cloud](https://www.terraform.io/cloud) for this, because I don't want to have to do things by hand if I can avoid it, and TFC is cool. [Terraform Enterprise](https://www.hashicorp.com/products/terraform) also exists, and is almost the same thing as Terraform Cloud, but different in ways I'm not going to get into right now. If you do not have an account, you can sign up for one at [app.terraform.io](https://app.terraform.io/)

I'm also not going into how to set up Vault; this is mostly for my own reference, but if anybody else is following this, I'm assuming you've got a Vault set up already. If you haven't got a Vault yet, you may wish to consider HCP Vault, which is now [generally available](https://www.youtube.com/watch?v=B0-WO9p45HE), so you don't have to put much effort into spinning it up.

My Vault is available over the public Internet directly, because YOLO, so if yours isn't then you'll also need to get routing from TFC/TFE to Vault in place.

Let's begin!

# Vault Bootstrapping

Let's start with the bootstrap configuration on the Vault side.

First thing we need is a Policy, which will grant Terraform the ability to manage stuff. For now, I'm leaving it at the bare minimum, just so I can prove that it's all working, and I'll expand on it later (with Terraform itself).

My initial `terraform_vault` policy looks like this:

```
# Terraform creates a Child Token to interact with Vault
path "auth/token/create" {
  capabilities = ["update"]
}

# A Test secret, to prove TF is working
# In my case, this is a KVv2 Secret Engine, so we need /data/ in there
path "kv/data/terraform" {
  capabilities = ["create", "update", "read"]
}
```

Next, we need an Auth method. I'll be using [AppRole](https://www.vaultproject.io/docs/auth/approle) for this, because it's super flexible, and nobody's written a dedicated Terraform Cloud/Terraform Enterprise Auth plugin for Vault yet[^3].

Logged in to my Vault UI, I'm going to use the built-in Browser CLI. That's this little icon in the top right:

![Vault Browser CLI icon](/images/posts/2021-04-07/cli.png)

I'd love to this in the UI directly... but there's no AppRole UI yet. Maybe one day.

So, again, bare minimum:

```
> write auth/approle/role/vault_terraform token_policies=vault_terraform token_ttl=300

Success! Data written to: auth/approle/role/vault_terraform

> read auth/approle/role/vault_terraform

Key                     Value              
bind_secret_id          true               
local_secret_ids        false              
secret_id_bound_cidrs   null               
secret_id_num_uses      0                  
secret_id_ttl           0                  
token_bound_cidrs       []                 
token_explicit_max_ttl  0                  
token_max_ttl           0                  
token_no_default_policy false              
token_num_uses          0                  
token_period            0                  
token_policies          ["vault_terraform"]
token_ttl               300                
token_type              default                
```

Short TTL, no usage limits or CIDR restrictions, etc. But we're bootstrapping, and this isn't Production, so we don't care right now. Terraform will update it's own AppRole later to make it more secure.


# Terraform Code

Now we need some Terraform code, to actually... you know... do something.

First thing, we tell Terraform we're using TFC:

```
terraform {
  backend "remote" {
    organization = "lmhd"

    workspaces {
      name = "vault"
    }
  }
}
```

This links the repo to a Terraform Cloud workspace (which we will create in a bit), and uses that to store the Terraform state.

Next we need to auth with Vault:

```
# These variables intentionally left blank
variable "login_approle_role_id" {}
variable "login_approle_secret_id" {}

provider "vault" {
  # Not setting Vault Address
  # We can pull that from the VAULT_ADDR env var

  auth_login {
    path = "auth/approle/login"

    parameters = {
      role_id   = var.login_approle_role_id
      secret_id = var.login_approle_secret_id
    }
  }
}
```

Here we're telling Terraform to use AppRole authentication to log in to Vault, and we're giving it the AppRole's Role ID and Secret ID as a Terraform Variable.

I'm not specifying where my Vault is; I'll be doing that with an environment variable.


And then, finally, we'll have Terraform actually _do_ something. In this case, create a simple KV secret:

```
resource "vault_generic_secret" "example" {
  path = "kv/terraform"

  data_json = <<EOT
{
  "foo":   "bar",
  "pizza": "cheese"
}
EOT
}
```

Git Commit, Git Push, Done.

We'll add a bunch more stuff to this later, but that's a problem for Future Lucy.


# Terraform Cloud

Now we want to set things up in TFC so it applies our Terraform code. There is a Terraform Provider for Terraform Cloud[^4], and I'll probably look into that in future. For now, I'll just do things by hand.

First things first, we need to create a new Workspace. I'm using the Version control workflow, so I can link it to a Git repo, and not have to worry about things:

![Terraform Cloud, Create a new Workspace UI, Step 1: Choose Type. Version control workflow is highlighted](/images/posts/2021-04-07/tfc-01-create-workspace.png)

In my case, my Terraform code is stored in GitHub, so I'll select that:

![Terraform Cloud, Create a new Workspace UI, Step 2: Connect to VCS. Options in this example are GitHub and BitBucket, with the option to connect to a different VCS](/images/posts/2021-04-07/tfc-02-connect-to-vcs.png)

And I'll pick my `vault_terraform` repo:

![Terraform Cloud, Create a new Workspace UI, Step 3: Choose a repository. Filtered to repos named "vault_", we see one repo: "vault_terraform"](/images/posts/2021-04-07/tfc-03-choose-repo.png)

As far as Settings go, I'm going to call my workspace "vault":

![Terraform Cloud, Create a new Workspace UI, Step 4: Settings. Workspace Name is "vault"](/images/posts/2021-04-07/tfc-04-configure-settings.png)

And I'm going to expand the Advanced options and enable Automatic speculative plans. This should mean that if I make Pull Requests into this repo, then Terraform Cloud will run `terraform plan` on them. Pretty useful.

![Terraform Cloud, Create a new Workspace UI, Step 4: Settings. Automatic speculative plans is enabled](/images/posts/2021-04-07/tfc-05-speculative-plans.png)

We're almost done!


# Terraform Cloud Vault Auth

Now that we have Terraform Cloud configured to use our Terraform code, we need to tell it how to find Vault, and how to auth.

We'll start with the latter.

Remember how, in the Terraform code, we defined some AppRole variables, but didn't set any values?

```
variable "login_approle_role_id" {}
variable "login_approle_secret_id" {}
```

We'll set those now.

Back in our Vault UI's Browser CLI, we can read the AppRole's Role ID with:

```
> read auth/approle/role/vault_terraform/role-id
```

And we can generate a Secret ID with:

```
> write -force auth/approle/role/vault_terraform/secret-id
```

In our Terraform Cloud Workspace, we can add these as Terraform Variables. These are secrets, so I recommend marking them as Sensitive, so they do not show up in the UI and cannot be read.

We can also add the address for our Vault as an Environment Variable, `VAULT_ADDR`.

![Terraform Cloud, Variables page. Two Terraform variables defined: login_approle_role_id and login_approle_secret_id. One environment variable set: VAULT_ADDR=https://vault.fancycorp.io](/images/posts/2021-04-07/tfc-06-variables.png)

And that should be everything.

# But does it work?

In the Runs tab, we can trigger a new plan:

![Terraform Cloud, Que plan manually dropdown](/images/posts/2021-04-07/tfc-07-trigger-plan.png)

I'm redirected to the Run details page for this Plan, and pretty quickly I can see that Terraform wants to make some changes:

```
Terraform v0.14.9
Configuring remote state backend...
Initializing Terraform configuration...

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # vault_generic_secret.example will be created
  + resource "vault_generic_secret" "example" {
      + data         = (sensitive value)
      + data_json    = (sensitive value)
      + disable_read = false
      + id           = (known after apply)
      + path         = "kv/terraform"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

```

I'm asked to confirm before it applies anything, and then we see that the Apply has been successful.

```
Terraform v0.14.9
vault_generic_secret.example: Creating...
vault_generic_secret.example: Creation complete after 1s [id=kv/terraform]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

And if we check in Vault, we can see that a secret has been created:

![Vault Secret, at kv/terraform. Data is foo=bar, pizza=cheese](/images/posts/2021-04-07/vault-01-secret.png)

And to make sure it's persisting state and reading from Vault, I'm going modify that secret by hand, then run Terraform again.

This time we see the plan:

```
Terraform v0.14.9
Configuring remote state backend...
Initializing Terraform configuration...
vault_generic_secret.example: Refreshing state... [id=kv/terraform]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # vault_generic_secret.example will be updated in-place
  ~ resource "vault_generic_secret" "example" {
      ~ data_json    = (sensitive value)
        id           = "kv/terraform"
        # (3 unchanged attributes hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.
```

So I'm satisfied that everything is working as it should.





# Next Steps

So it's pretty basic so far, but it's a good foundation to build from.

My next steps with this will be to Terraform the chicken/egg things. i.e. the `vault_terraform` Policy and AppRole, and then work my way through the rest of the configuration I already have.

I'm also going to see if I can dynamically determine the IP addresses for Terraform Cloud[^5], and add those as a CIDR restriction on the AppRole.

Should be fun!





[^1]: You can [watch my Vault Terraform HashiConf talk on YouTube](https://www.youtube.com/watch?v=vD3_jeqGx6M&list=PLdfwCbsYUpU-7VyTGuHf3iFHc3r6mtsPA&index=2), or [read it on the HashiCorp website](https://www.hashicorp.com/resources/how-we-accelerated-our-vault-adoption-with-terraform)

[^2]: At time of writing, Sky Betting & Gaming is my employer. We have a [technology blog](https://sbg.technology/) if you want to check it out

[^3]: oh no, don't give me ideas...

[^4]: [Terraform Cloud Terraform Provider](https://registry.terraform.io/providers/hashicorp/tfe/latest)

[^5]: HashiCorp have [an API to list Terraform Cloud IP ranges](https://www.terraform.io/docs/cloud/api/ip-ranges.html)