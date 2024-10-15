# Custom Default Backend for NGINX Ingress Controller

<p align="center">
  <a href="LICENSE" alt="GitHub License">
    <img src="https://img.shields.io/github/license/dvdblk/custom-default-backend?label=license"/>
  </a>
</p>

Customize the default backend for an NGINX Ingress Controller on your kubernetes cluster. A single Docker image with an NGINX server that handles most status codes.

[LIVE Preview - 404 status code](https://dvdblk.com/thispagedoesntexist)

[LIVE Preview - 403 status code](https://auth.dvdblk.com/thispageisforbiddenandalsodoesntexist)

## Installation via Helm
You can use the `defaultBackend` property of the [ingress-nginx](https://github.com/kubernetes/ingress-nginx/tree/main/charts/ingress-nginx) helm chart like this:

```
defaultBackend:
  enabled: true
  image:
    # Or your own forked one...
    repository: dvdblk/custom-default-backend
    tag: "latest"
    pullPolicy: Always
  port: 8080
  extraVolumeMounts:
    - name: tmp
      mountPath: /tmp
  extraVolumes:
    - name: tmp
      emptyDir: {}
```

## HTML Customization

Fork this repository and edit the HTML inside the `content/` directory if you want some CSS / text customization.

If you wish to have a different HTML error page per status code you can omit the `ssi` part and set up the error pages (e.g. `content/404.html`, `content/500.html`) like this:

```
# default.conf

server {
  # ...

  error_page 400 /404.html;
  location = /404.html {
    internal;
  }

  error_page 500 /500.html;
  location = /500.html {
    internal;
  }

  # ...
}
```
