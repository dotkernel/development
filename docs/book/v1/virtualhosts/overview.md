# AlmaLinux 9 Overview

Virtualhosts allow developers to host multiple applications on their local system.

Using this tool, you configure a virtualhost for each of your applications, and it will create them so that you can start working with them.

**Example**:

* `api.dotkernel.localhost`: this could be the endpoint where you host your website's API
* `frontend.dotkernel.localhost`: this could be the subdomain where you host your website's frontend that will consume the API

In the above example, the URLs are built like this:

* the subdomain is the identifier of your application (`api` / `frontend`)
* the domain is the identifier of your project (`dotkernel`)
* the TLD sends the requests to localhost where Apache will route them to their real location

> By using the pattern `*.localhost` for any new virtualhost, you do not need to modify the `hosts` file in Windows, because these are routed by default.
