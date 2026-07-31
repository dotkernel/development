# AlmaLinux 10 Overview

## Summary

Virtualhosts let you host multiple local applications under `*.localhost` subdomains, routed automatically through Apache without editing the Windows `hosts` file.

Virtualhosts allow developers to host multiple applications on their local system.

Using this tool, you configure a virtualhost for each of your applications, and it will create them so that you can start working with them.

**Example**:

* `api.dotkernel.localhost`: this could be the endpoint where you host your website's API
* `frontend.dotkernel.localhost`: this could be the subdomain where you host your website's frontend that will consume the API

In the above example, the URLs are built like this:

* The subdomain is the identifier of your application (`api` / `frontend`).
* The domain is the identifier of your project (`dotkernel`).
* The TLD sends the requests to localhost where Apache will route them to their real location.

> By using the pattern `*.localhost` for any new virtualhost, you do not need to modify the `hosts` file in Windows, because these are routed by default.

## Next step

Ready to set one up?
Continue to [Create virtualhost](create-virtualhost.md).

## FAQ

**Q: Do I need to edit the Windows `hosts` file for my virtualhosts?**

A: No, as long as you use the `*.localhost` pattern (for example `api.dotkernel.localhost`), these domains are routed automatically and require no `hosts` file changes.

**Q: Can I use a domain that doesn't end in `.localhost`?**

A: The automatic routing described here relies on the `*.localhost` pattern.
Using a different TLD would require manually editing the Windows `hosts` file yourself.

**Q: How are the subdomain and domain parts of a virtualhost URL structured?**

A: The subdomain identifies the application (for example `api` or `frontend`), and the domain identifies the project (for example `dotkernel`) - together with the `.localhost` TLD, Apache routes the request to the right place.

**Q: Where do I actually create a virtualhost?**

A: This page only covers the concept.
See [Create virtualhost](create-virtualhost.md) for the steps to provision one.
