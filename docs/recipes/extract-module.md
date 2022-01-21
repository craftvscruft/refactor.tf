---
layout: default
title:  "Extract Module"
parent: Refactoring Patterns
categories: refactor extract
---
# Extract Module
## Procedure

Extracting a module, let's call it M, from a config C.

Start with a list of resource and/or data blocks that you'd like to extract.

1. Create module folder with a `main.tf`, `variables.tf`, `outputs.tf`
2. Include the new empty module from config C with a `module` block
3. Move the extracted elements from C to M's `main.tf`
4. Add `moved` blocks to prevent state changes
5. For each reference in M to elements in C
    1. Add a variable to M's `variables.tf`
    2. Move the expression to an attribute in C's module include
    3. Replace the expression in M with a reference to the variable
6. For each reference in C to elements in M
    1. Add it as an output to M's `outputs.tf`
    2. Change the reference in C to use the module's output.
7. Verify Empty Plan

## Automation

See tfrefactor.
```
tfrefactor extract -t ../mymodule my_resource.a my_resource.b
```

## Source code
Before
{% highlight terraform %}
# C/main.yml
locals {
  a_local = "local_val"
}
resource "my_resource" "a" {
}
resource "my_resource" "b" {
  a_val = local.a_local
}
resource "something" "else" {
  attribute = my_resource.a.attr
}
{% endhighlight %}

After
{% highlight terraform %}
# M/main.tf
resource "my_resource" "a" {
}
resource "my_resource" "b" {
  a_val = vaar.b_a_val
}

# M/variables
variable "b_a_val" {
}

# M/outputs.tf
output "a_attr" {
  value = my_resource.a.attr
}

# C/main.tf
locals {
  a_local = "local_val"
}
resource "something" "else" {
  attribute = module.m.a_attr
}
module "m" {
  source = "../M"
}
moved {
  from my_resource.a
  to module.m.my_resource.a
}
moved {
  from my_resource.b
  to module.m.my_resource.b
}
{% endhighlight %}

## State operations
If not using [moved blocks](https://learn.hashicorp.com/tutorials/terraform/move-config) introduced in Terraform 1.1, a `state mv` operation must be used to avoid destroying and recreating resources.

{% highlight bash %}
terraform state mv my_resource.a module.m.my_resource.a
terraform state mv my_resource.b module.m.my_resource.b
{% endhighlight %}

## See also.

* Move resource to module
* Rename resource
* Rename output
* Rename variable
