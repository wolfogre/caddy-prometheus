# Metrics

This module enables prometheus metrics for Caddy.

## Use

In your `Caddyfile`:

~~~
prometheus
~~~

For each virtual host that you want to see metrics for.

These are the (optional) parameters that can be used:

  - **use_caddy_addr** - causes metrics to be exposed at the same address:port as Caddy itself. This can not be specified at the same time as **address**.
  - **address** - the address where the metrics are exposed, the default is `localhost:9180`
  - **path** - the path to serve collected metrics from, the default is `/metrics`
  - **hostname** - the `host` parameter that can be found in the exported metrics, this defaults to the label specified for the server block
  - **label** - Custom label to add on all metrics.
    This directive can be used multiple times.  
    You should specify a label name and a value.  
    The value is a [placeholder](https://caddyserver.com/docs/placeholders) and can be used to extract value from response header for instance.  
    Usage: `label route_name {<X-Route-Name}`

With `caddyext` you'll need to put this module early in the chain, so that
the duration histogram actually makes sense. I've put it at number 0.

## Metrics

The following metrics are exported:

* caddy_http_request_count_total{host}
* caddy_http_request_duration_seconds{host}
* caddy_http_response_latency_seconds{host status}
* caddy_http_response_size_bytes{host, status}
* caddy_http_response_status_count_total{host, status}

Each metric has the following labels:

* `host` which is the hostname used for the request/response,

The `response_*` metrics have an extra label `status` which holds the status code.
