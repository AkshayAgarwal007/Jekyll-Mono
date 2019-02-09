---
layout: page
title: About
permalink: /about/
---

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam imperdiet urna eu dolor placerat varius. Vivamus eros augue, consequat id scelerisque nec, fringilla in est. Proin pellentesque malesuada mauris, quis aliquam augue vestibulum ac. Vestibulum ut feugiat nibh. Sed faucibus felis purus, sed convallis leo dictum vehicula. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam imperdiet urna eu dolor placerat varius. Vivamus eros augue, consequat id scelerisque nec, fringilla in est. Proin pellentesque malesuada mauris, quis aliquam augue vestibulum ac. Vestibulum ut feugiat nibh. Sed faucibus felis purus, sed convallis leo dictum vehicula.  

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam imperdiet urna eu dolor placerat varius. Vivamus eros augue, consequat id scelerisque nec, fringilla in est. Proin pellentesque malesuada mauris, quis aliquam augue vestibulum ac. Vestibulum ut feugiat nibh. Sed faucibus felis purus, sed convallis leo dictum vehicula. 

Sed faucibus felis purus, sed convallis leo dictum vehicula.

## Education

* ABC degree from XYZ University.
* ABC degree from XYZ University.
* ABC degree from XYZ University.

## Roles

Founder, ABC Organisation

## Skills

* **Skill 1** - `Skill` / `Skill` / `Skill` / `Skill`
* **Skill 2** - `Skill` / `Skill` / `Skill` / `Skill` / `Skill` / `Skill` / `Skill`
* **Skill 3** - `Skill` / `Skill` / `Skill`
* **Skill 4** - `Skill` / `Skill` / `Skill` 
* **Skill 5** - `Skill`
* **Skill 6** - `Skill` / `Skill` 
    
    
## Achievements


* [**This is my first achievement**](#) 
   
   Proin pellentesque malesuada mauris, quis aliquam augue vestibulum ac. Vestibulum ut feugiat nibh. Sed faucibus felis purus, sed convallis leo dictum vehicula.

***

* [**This is my second achievement**](#) 

    Proin pellentesque malesuada mauris, quis aliquam augue vestibulum ac. Vestibulum ut feugiat nibh. Sed faucibus felis purus, sed convallis leo dictum vehicula.

***

* [**This is my third achievement**](#) 

   Proin pellentesque malesuada mauris, quis aliquam augue vestibulum ac. Vestibulum ut feugiat nibh. Sed faucibus felis purus, sed convallis leo dictum vehicula


## Contact me

[agarwal.akshay.akshay8@gmail.com](mailto:agarwal.akshay.akshay8@gmail.com)


{% include base_path %}
{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
