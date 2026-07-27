---
title: AlmaLinux 10 Overview
description: Virtualhosts let you host multiple local applications under *.localhost subdomains, routed automatically through Apache without editing the Windows hosts file.
author: "admin"
date_published: "2026-07-27"
canonical_url: "https://docs.dotkernel.org/development/v2/virtualhosts/overview"
category: "Development"
language: "en"
---

# AlmaLinux 10 Overview

## TL;DR

Virtualhosts let you host multiple local applications under `*.localhost` subdomains, routed automatically through Apache without editing the Windows `hosts` file. For example, `api.dotkernel.localhost` and `frontend.dotkernel.localhost` can host an API and its consuming frontend side by side on the same machine.

## FAQ

### Do I need to edit the Windows `hosts` file for my virtualhosts?

No, as long as you use the `*.localhost` pattern (for example `api.dotkernel.localhost`), these domains are routed automatically and require no `hosts` file changes.

### Can I use a domain that doesn't end in `.localhost`?

The automatic routing described here relies on the `*.localhost` pattern.
Using a different TLD would require manually editing the Windows `hosts` file yourself.

### How are the subdomain and domain parts of a virtualhost URL structured?

The subdomain identifies the application (for example `api` or `frontend`), and the domain identifies the project (for example `dotkernel`) - together with the `.localhost` TLD, Apache routes the request to the right place.

### Where do I actually create a virtualhost?

This page only covers the concept.
See [Create virtualhost](create-virtualhost.md) for the steps to provision one.
