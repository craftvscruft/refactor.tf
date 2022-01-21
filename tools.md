---
layout: post
title:  "Tools"
date:   2022-01-17 22:48:59 -0500
categories: tools 
---

# Tools For Refactoring Terraform
## Grep

Usually in the form of [git grep](https://git-scm.com/book/en/v2/Git-Tools-Searching). Useful for scanning a codebase for references when considering a rename or other change. In the case of cross-repo dependencies, consider an org-wide [GitHub search](https://docs.github.com/en/github/searching-for-information-on-github/searching-on-github/searching-code#search-within-a-users-or-organizations-repositories) or [equivelent](https://support.atlassian.com/bitbucket-cloud/docs/search-in-bitbucket-cloud/).

## Built in
* terraform plan - Confirm a change is non-disruptive with an [empty plan]({% link docs/recipes/verify-empty-plan.md %})
* terraform state mv
* [Moved blocks](https://learn.hashicorp.com/tutorials/terraform/move-config) introduced in Terraform 1.1
* terraform import - Bring an unmanaged resource into your terraform state. Useful for legacy infrastructure or transferring from one state to another.
* terraform validate
* terraform fmt - Format code according to Hashicorp standard.
* local state - Useful for trying out state operations before applying.
* [provider documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) - In addition to resource arguments, resource attributes and data sources are also useful.

## Migration support

[tfmigrate](https://github.com/minamijoyo/tfmigrate) is a Terraform state migration tool for GitOps. 


## Static checks
* [tflint](https://github.com/terraform-linters/tflint) - Linter checking for possible issues including errors, deprecated syntax, and naming conventions
* [tfsec](https://github.com/aquasecurity/tfsec) - Security scanner for your Terraform code

## Testing

[tdd-infrastructure](https://github.com/joatmon08/tdd-infrastructure) is a compilation of terraform testing tools and examples.





