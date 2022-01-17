---
layout: post
title:  "Verify Empty Terraform Plan"
date:   2021-08-25 22:48:59 -0500
categories: tools testing
---

## Built in
* terraform plan

## Showing changes when nothing has changed
"config refers to values not yet known"

``` terraform
block {
    ~ attr = "old value" -> (known after apply)
}
```

This can happen when your resource depends on an ['external' data source](https://registry.terraform.io/providers/hashicorp/external/latest/docs/data-sources/data_source), directly or indirectly.
