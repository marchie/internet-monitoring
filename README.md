# Internet Monitoring

A Docker Compose stack for monitoring the availability of internet-based services.

The stack includes:

- [Prometheus](https://prometheus.io): an open-source monitoring system with a dimensional data model, flexible query 
  language, efficient time series database and modern alerting approach;
- [Blackbox Exporter](https://github.com/prometheus/blackbox_exporter): to enable blackbox probing of endpoints over 
  HTTP, HTTPS, DNS, TCP, ICMP and gRPC; and
- [Grafana](https://grafana.com): an open source analytics and interactive visualization web application.

## Usage

To start the stack, run:

```shell
docker compose up -d
```

## Component Configuration

- [Prometheus](./prometheus/prometheus.yml): endpoint probe configuration (hosts, type of probe, frequency)
- [Blackbox Exporter](./blackbox/config/blackbox.yml): Blackbox module configuration (timeouts, etc.)
- [Grafana](./grafana/config.monitoring): user interface admin password

## Backstory

We recently had fibre-to-the-premises (FTTP) installed at our home. The FTTP service promises to provide speeds of 
1Gb/sec down and 200Mb/sec up, which compares very favourably with our existing fibre-to-the-cabinet connection, which
only provides around 40Mb/sec down and 6Mb/sec up.

The router provided by the service provider seems to be pretty poor, so I elected to plug in a Unifi Dream Machine
(UDM) from day one.

However, it's not been a great experience. We have had the promised speeds... but we have not had a reliable 
_connection_. We have found that our connection seems to periodically drop out for a short period of time every
few hours. This is particularly annoying for video calls, working on a remote desktop, video streaming, etc.

The Unifi Dream Machine has processes which monitor the availability and quality of the internet connection, by pinging
various endpoints that are assumed to be highly available (i.e. Google, CloudFlare). These monitors report short
outages that seem to occur quite randomly.

However - I need to determine whether the cause of these outages is the Unifi Dream Machine itself, or whether it's
something that's happening on our Internet Service Provider's systems.

So - I'm going to plug the ISP-provided router back in - and I'm going to monitor the quality of the connection
by physically connecting a computer to the router via Ethernet and running this stack on the computer. If we see
the same kind of outages reported by this stack, then it indicates that the problem is on the ISP's end rather than
ours...!

## Credit

This repository is based on [maxandersen/internet-monitoring](https://github.com/maxandersen/internet-monitoring/).
