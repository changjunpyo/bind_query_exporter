# BIND Query log Prometheus Exporter

A [Prometheus](https://prometheus.io) exporter for BIND queries. This exporter consumes the BIND9 query log file. It is based on the [node_exporter](https://github.com/prometheus/node_exporter) and [cf_exporter](https://github.com/bosh-prometheus/cf_exporter) projects.

## Installation

### Binaries

Download the already existing [binaries](https://github.com/DRuggeri/bind_query_exporter/releases) for your platform:

```bash
$ ./bind_query_exporter <flags>
```

### From source

Using the standard `go install` (you must have [Go](https://golang.org/) already installed in your local machine):

```bash
$ go install github.com/DRuggeri/bind_query_exporter
$ bind_query_exporter <flags>
```

## Usage

### Flags

```
usage: bind_query_exporter [<flags>]

Flags:
  -h, --help              Show context-sensitive help (also try --help-long and --help-man).
      --log="/var/log/bind/queries.log"
                          Path of the BIND query log to watch. Defaults to '/var/log/bind/queries.log' ($BIND_QUERY_EXPORTER_LOG)
      --filter.collectors="Stats, Sites"
                          Comma separated collectors to enable (Stats,Sites) ($BIND_QUERY_EXPORTER_FILTER_COLLECTORS)
      --metrics.namespace="bind_query"
                          Metrics Namespace ($BIND_QUERY_EXPORTER_METRICS_NAMESPACE)
      --web.listen-address=":9197"
                          Address to listen on for web interface and telemetry ($BIND_QUERY_EXPORTER_WEB_LISTEN_ADDRESS)
      --web.telemetry-path="/metrics"
                          Path under which to expose Prometheus metrics ($BIND_QUERY_EXPORTER_WEB_TELEMETRY_PATH)
      --web.auth.username=WEB.AUTH.USERNAME
                          Username for web interface basic auth ($BIND_QUERY_EXPORTER_WEB_AUTH_USERNAME)
      --web.auth.password=WEB.AUTH.PASSWORD
                          Password for web interface basic auth ($BIND_QUERY_EXPORTER_WEB_AUTH_PASSWORD)
      --web.tls.cert_file=WEB.TLS.CERT_FILE
                          Path to a file that contains the TLS certificate (PEM format). If the certificate is signed by a certificate authority, the file should be the concatenation of the server's certificate, any intermediates, and the CA's certificate
                          ($BIND_QUERY_EXPORTER_WEB_TLS_CERTFILE)
      --web.tls.key_file=WEB.TLS.KEY_FILE
                          Path to a file that contains the TLS private key (PEM format) ($BIND_QUERY_EXPORTER_WEB_TLS_KEYFILE)
      --printMetrics      Print the metrics this exporter exposes and exits. Default: false ($BIND_QUERY_EXPORTER_PRINT_METRICS)
      --log.level="info"  Only log messages with the given severity or above. Valid levels: [debug, info, warn, error, fatal]
      --log.format="logger:stderr"
                          Set the log target and format. Example: "logger:syslog?appname=bob&local=7" or "logger:stdout?json=true"
      --version           Show application version.
```

## Metrics

### Stats
This collector counts the number of DNS queries the DNS server receives by type.

```
  bind_query_stats_total - Total queries recieved
  bind_query_stats_total_by_type - Total queries recieved by type of query
  bind_query_stat_scrapes_total - Total number of scrapes for BIND query stats.
  bind_query_stats_scrape_errors_total - Total number of scrapes errors for BIND query stats.
  bind_query_last_stat_scrape_error - Whether the last scrape of BIND query stats resulted in an error (1 for error, 0 for success).
  bind_query_last_stat_scrape_timestamp - Number of seconds since 1970 since last scrape of BIND qyery stat metrics.
  bind_query_last_stat_scrape_duration_seconds - Duration of the last scrape of BIND query stats.
```

### Sites
This collector counts unique hits to individual DNS names.

**IMPORTANT NOTE:** Each DNS name gets its own label and counter. This causes the cardinality problems mentioned [here](https://prometheus.io/docs/practices/instrumentation/#do-not-overuse-labels) and [here](https://prometheus.io/docs/practices/naming/#labels) if your nameserver is used as a recursive server or sees hits for many domains!

```
  bind_query_site_number - Queries per DNS name
  bind_query_site_scrapes_total - Total number of scrapes for BIND sites stats.
  bind_query_site_scrape_errors_total - Total number of scrapes errors for BIND sites stats.
  bind_query_last_site_scrape_error - Whether the last scrape of BIND sites stats resulted in an error (1 for error, 0 for success).
  bind_query_last_site_scrape_timestamp - Number of seconds since 1970 since last scrape of BIND sites metrics.

```

## Contributing

Refer to the [contributing guidelines](https://github.com/DRuggeri/bind_query_exporter/blob/master/CONTRIBUTING.md).

## License

Apache License 2.0, see [LICENSE](https://github.com/DRuggeri/bind_query_exporter/blob/master/LICENSE).