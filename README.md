# HAProxy Ingress controller

[Ingress](https://kubernetes.io/docs/user-guide/ingress/) controller implementation for [HAProxy](http://www.haproxy.org/) loadbalancer.

[![Build Status](https://travis-ci.org/jcmoraisjr/haproxy-ingress.svg?branch=master)](https://travis-ci.org/jcmoraisjr/haproxy-ingress) [![Docker Repository on Quay](https://quay.io/repository/jcmoraisjr/haproxy-ingress/status "Docker Repository on Quay")](https://quay.io/repository/jcmoraisjr/haproxy-ingress)

# Releases

HAProxy Ingress images are built by [Travis CI](https://travis-ci.org/jcmoraisjr/haproxy-ingress) and the
image is deployed from Travis CI to [Quay.io](https://quay.io/repository/jcmoraisjr/haproxy-ingress?tag=latest&tab=tags)
whenever a tag is applied. The `latest` tag will always point to the latest stable version while
`canary` tag will always point to the latest deployed version.

# Usage

Usage docs are maintained on Ingress repository:

* Start with [deployment](https://github.com/kubernetes/ingress/tree/master/examples/deployment/haproxy) instructions
* See [TLS termination](https://github.com/kubernetes/ingress/tree/master/examples/tls-termination/haproxy) on how to enable `https`

# Configuration

HAProxy Ingress can be configured per ingress resource using annotations, or globally
using ConfigMap. It is also possible to change the default template mounting a new
template file at `/usr/local/etc/haproxy/haproxy.tmpl`.

## Annotations

The following annotations are supported:

|Name|Data|Usage|
|---|---|:---:|
|`ingress.kubernetes.io/auth-type`|"basic"|[doc](https://github.com/kubernetes/ingress/tree/master/examples/auth/basic/haproxy)|
|`ingress.kubernetes.io/auth-secret`|secret name|[doc](https://github.com/kubernetes/ingress/tree/master/examples/auth/basic/haproxy)|
|`ingress.kubernetes.io/auth-realm`|realm string|[doc](https://github.com/kubernetes/ingress/tree/master/examples/auth/basic/haproxy)|
|`ingress.kubernetes.io/auth-tls-secret`|namespace/secret name|[doc](https://github.com/kubernetes/ingress/tree/master/examples/auth/client-certs/haproxy)|
|`ingress.kubernetes.io/ssl-redirect`|[true\|false]|[doc](https://github.com/kubernetes/ingress/tree/master/examples/rewrite/haproxy)|
|`ingress.kubernetes.io/app-root`|/url|[doc](https://github.com/kubernetes/ingress/tree/master/examples/rewrite/haproxy)|
|`ingress.kubernetes.io/whitelist-source-range`|CIDR|-|

## ConfigMap

If using ConfigMap to configure HAProxy Ingress, use
`--configmap=<namespace>/<configmap-name>` argument on HAProxy Ingress deployment.
A ConfigMap can be created with `kubectl create configmap`.

The following parameters are supported:

|Name|Type|Default|
|---|---|---|
|[`ssl-redirect`](#ssl-redirect)|[true\|false]|`true`|
|[`syslog-endpoint`](#syslog-endpoint)|IP:port (udp)|do not log|

### ssl-redirect

A global configuration of SSL redirect used as default value if ingress resource
doesn't use `ssl-redirect` annotation. If true HAProxy Ingress sends a `302 redirect`
to https if TLS is configured.

### syslog-endpoint

Configure the UDP syslog endpoint where HAProxy should send access logs.
