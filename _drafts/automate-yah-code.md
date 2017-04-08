---
layout:      post
title:       What to do about code pipelines?
author:      Lucy
date:        1970-01-01 00:00:00
category:    tech
tags:        [terraform, continuous-delivery]
---


Doing stuff manually is dumb. That's part of the reason why I'm starting to Terraform some stuff that was previously manually configured. However, that alone is not good enough. I still have to run `terraform plan` and `terraform apply` myself[^1], or run other commands for stuff hosted elsewhere.

[^1]: or `make` and `make apply` in my case, based on [this example](TODO)

That's not really a big deal. It's only me editing this stuff, so it's easy enough for me to deploy it manually. But my day job is literally to Automate All The Things. It therefore feels wrong to do something manually when there are better options.

So what to do?

I need something generic, which I can use for many things. It needs to be easy to use. It needs pipelines to be defined as code. And it needs to be free, or at the very least cheap.

## Jenkins

Jenkins is my boyfriend. I'm sure we will have sweet CI/CD funtimes before too long.

It fits all of my requirements:

* It's super generic, and configurable to the ends of the earth with a massive number of plugins,
* I've been using it for years, so I know it well enough to consider it easy to use,
* It has first class pipeline-as-code support, which has recently been improved with [BlueOcean](TODO)
* It's Open Source, and free to use,

But I would need to figure out where to host it.

### OpenShift

https://www.openshift.com

I actually already have a Jenkins set up and running here. I set it up a year or so ago when I wanted to play around with some stuff. I'm not paying anything for it, which is probably why it stays online for a few days then dies a death.

### CloudBees Hosted

https://www.cloudbees.com/products

https://www.cloudbees.com/products/jenkins-cloud

### RPi?

Because why not?

### Heroku?

TODO

## Travis

TODO

https://travis-ci.com/plans

## CircleCI

TODO

https://circleci.com/pricing/

## other similar tools

TODO + look around

## CodeShip

Already use this for some things

## CodePipeline

https://www.ruempler.eu/2017/02/26/continuous-infrastructure-delivery-pipeline-aws-codepipeline-codebuild-terraform/

$1 per active pipeline per month

TODO

## BitBucket Pipelines

TODO

## Does GitHub have webooks for stuff?

TODO

## Lambda functions? Ad-hoc Heroku Dynos? Something custom?

TODO
