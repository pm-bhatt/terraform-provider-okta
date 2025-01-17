---
page_title: "Data Source: okta_org_metadata"
description: |-
  Retrieves the well-known org metadata, which includes the id, configured custom domains, authentication pipeline, and various other org settings.
---

# Data Source: okta_org_metadata

Retrieves the well-known org metadata, which includes the id, configured custom domains, authentication pipeline, and various other org settings.

## Example Usage
```terraform
data "okta_org_metadata" "_" {}
locals {
  org_url = try(
    data.okta_org_metadata._.alternate,
    data.okta_org_metadata._.organization
  )
}

resource "okta_resource_set" "iam_resources" {
  label       = "iam_resources"
  description = "iam_resources"
  resources = [
    "orn:okta:iam:${data.okta_org_metadata._.id}:contained_resources"
  ]
}

resource "okta_resource_set" "support_resources" {
  label       = "support_resources"
  description = "support_resources"
  resources = [
    "orn:okta:support:${data.okta_org_metadata._.id}:cases"
  ]
}

resource "okta_resource_set" "example" {
  label       = "All users, groups, and apps"
  description = "All users, groups, and apps"
  resources = [
    "${local.org_url}/api/v1/users",
    "${local.org_url}/api/v1/apps",
    "${local.org_url}/api/v1/groups"
  ]
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Read-Only

- `domains` (Block, Read-only) The URIs for the org's configured domains. (see [below for nested schema](#nestedblock--domains))
- `id` (String) The unique identifier of the Org.
- `pipeline` (String) The authentication pipeline of the org. idx means the org is using the Identity Engine, while v1 means the org is using the Classic authentication pipeline.
- `settings` (Block, Read-only) The wellknown org settings (safe for public consumption). (see [below for nested schema](#nestedblock--settings))

<a id="nestedblock--domains"></a>
### Nested Schema for `domains`

Read-Only:

- `alternate` (String) Custom Domain Org URI
- `organization` (String) Standard Org URI


<a id="nestedblock--settings"></a>
### Nested Schema for `settings`

Read-Only:

- `analytics_collection_enabled` (Boolean)
- `bug_reporting_enabled` (Boolean)
- `om_enabled` (Boolean) Whether the legacy Okta Mobile application is enabled for the org


