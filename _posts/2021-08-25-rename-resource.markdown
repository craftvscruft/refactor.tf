---
layout: post
title:  "Rename Resource"
date:   2021-08-25 22:48:59 -0500
categories: refactor rename 
---

## Procedure

1. Rename block.
2. Rename all references within the module.
3. Move state from old name to new.
4. Verify empty plan.

## Source code
Before
{% highlight terraform %}
resource "my_resource" "old" {
}

resource "something" "else" {
  attribute = my_resource.old
}
{% endhighlight %}

After
{% highlight terraform %}
resource "my_resource" "new" {
}

resource "something" "else" {
  attribute = my_resource.new
}

moved {
  from my_resource.old
  to my_resource.new
}
{% endhighlight %}
## State operations
If not using [moved blocks](https://learn.hashicorp.com/tutorials/terraform/move-config) introduced in Terraform 1.1, a `state mv` operation must be used to avoid destroying and recreating the resource.

{% highlight bash %}
terraform state mv my_resource.old my_resource.new
{% endhighlight %}
Or, with [tfmigrate](https://github.com/minamijoyo/tfmigrate):
{% highlight hcl %}
migration "state" "rename" {
  dir = "."
  actions = [
    "mv my-resource.old my-resource.new"
  ]
}
{% endhighlight %}

## See also.

* Rename resource array. 
* Move resource into module.
* Move resource out of module.
* Rename variable. 
* Rename local. 
* Rename data.
