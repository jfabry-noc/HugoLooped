---
title: "Terraform Cloud Variables Are All Strings"
date: 2022-07-05T18:19:08-04:00
draft: false
tags:
  - Terraform
  - Terraform Cloud
---

This post easily falls into the “so ridiculous that it shouldn’t be a post” category. However, considering how long I ended up stuck on it earlier today, I figure it’s worth making if it stands a chance of helping even one other person avoid the same fate as me. 😅

I’ve started doing a little bit of work with the Terraform Cloud API. Specifically, I was looking at being able to update a [workspace variable](https://www.terraform.io/cloud-docs/api-docs/workspace-variables) through code. In this case, I was trying to change a variable I called `replica_count` which, if the name wasn’t enough of a giveaway, I was using to track how many replicas should be used in a Kubernetes container spec.

I found that I was able to easily get back all of my workspace variables via a `GET` to:

```
https://app.terraform.io/api/v2/workspaces/{workspace_id}/vars/
```

However, trying to make a `PATCH` to...

```
https://app.terraform.io/api/v2/workspaces/{workspace_id}/vars/{variable_id}
```

... with a body in JSON to specify the change resulted in a 500 internal server error response.

The JSON in the body wasn’t very complex. I only needed to modify the value. As the documentation specifies, and the `type` must be set to “vars” and the `id` must be set to the ID of the variable to change. This is a bit odd to me since I _also_ have to give the ID in the URL, but it’s not a big deal. The `attributes` property then can optionally contain whatever property or properties need to be updated. In my case, the JSON just looked like:

```
{
    “data”: {
        “id”: var_id,
        “type”: “vars”,
        “attributes”: {
            “value”: 2
        }
    }
}
```

There isn’t a lot to go wrong here... but I managed to do it! I finally figured out what was wrong when I changed my `PATCH` to a `GET` just to validate that my JSON had to be the culprit. The `GET` responds with JSON which is _very_ similar to what I needed to send, and that’s what tipped me off to the fact that the value of the variable is **always** a string, even if I’m thinking of it as a number in my head. So my `value` line needed to be:

```
“value”: “2”
```

After seeing that in the response to my `GET`, I updated my JSON, re-`PATCH`ed, and saw that everything was successful.
