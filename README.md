<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/icon-dark.svg">
    <img src="assets/icon.svg" alt="Checkmk logo" width="128">
  </picture>
</p>

<h1 align="center">aac-checkmk</h1>

<p align="center">Self-hosted deployment definition for Checkmk Raw, an open-source IT infrastructure monitoring platform.</p>

<p align="center">
  <a href="https://github.com/checkmk/checkmk"><img alt="Upstream" src="https://img.shields.io/badge/upstream-checkmk%2Fcheckmk-blue?logo=github"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-checkmk%2Fcheck--mk--raw%3A2.4.0--daily-2496ED?logo=docker&logoColor=white">
  <a href="https://github.com/checkmk/checkmk"><img alt="License" src="https://img.shields.io/badge/license-GPL--2.0-green"></a>
</p>

## About

Checkmk is an IT monitoring platform that discovers hosts and services automatically and
checks their state, performance and availability. The Raw Edition is the free, open-source
variant. It runs as an OMD site (Open Monitoring Distribution) that bundles Nagios-compatible
core monitoring, a web interface and an event console. Alerts and notifications are configured
inside the site.

Key features (as provided by upstream):

- Automatic host and service discovery with preconfigured checks
- Web-based configuration and dashboards (Multisite GUI)
- Livestatus query interface for integrations and scripts
- Event console for processing syslog and SNMP trap events
- Notification rules for alerting by e-mail and other channels

## Upstream

- Project: <https://github.com/checkmk/checkmk>
- Website: <https://checkmk.com>
- Documentation: <https://docs.checkmk.com>
- Container image: `checkmk/check-mk-raw:2.4.0-daily` (Docker Hub)
- License: GPL-2.0 (Raw Edition, upstream project)

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine. For a
manual run, the external networks used for routing must exist first:

```bash
docker network create services-secured
docker network create services-exposed

# Provide the values listed under "Configuration" in .env / stack.env
docker compose up -d
```

The container creates its monitoring site on first start. The site data is kept in the
`sites` volume, so it survives container re-creation.

Secrets are not stored in this repository. Provide them through the deployment pipeline
or create the `.env` and `stack.env` files yourself (see below). Do not commit them.

## Configuration

Environment variables (defaults from `service.yml`; `CHANGE_ME` must be set per deployment):

| Variable | Default | Description |
|---|---|---|
| `CMK_PASSWORD` | `CHANGE_ME` (secret) | Initial password of the `cmkadmin` web account |
| `OMD_SITE_ID` | `cmk` | ID of the OMD monitoring site inside the container |
| `CMK_SITE_NAME` | set per deployment | Display name of the monitoring site |
| `TZ` | `Europe/Berlin` | Time zone |
| `PUID` | `1000` | User ID the site runs as |
| `PGID` | `1000` | Group ID the site runs as |

Ports:

| Container port | Host port (default) | Protocol | Purpose |
|---|---|---|---|
| `5000` | `8285` | TCP | Web interface (`web`) |
| `5001` | `8286` | TCP | Livestatus query interface (`livestatus`) |
| `8000` | `8012` | TCP | Event console (`event-console`) |

Volumes:

| Mount | Description |
|---|---|
| `sites` -> `/omd/sites` | Checkmk site data and configuration |
| `/etc/localtime` -> `/etc/localtime` (read-only host time zone) | Keeps the container clock in line with the host |

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, environment,
   volumes and per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` (or a configured stage branch) runs the service pipeline, which renders
   the manifests with `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-checkmk>.

## Credits and license

Checkmk is developed by Checkmk GmbH and the Checkmk community and licensed under GPL-2.0 for
the Raw Edition. This repository only contains the deployment definition for it and is not a
fork of the application. The logo is provided by the selfh.st icon collection; see
[`assets/ICON_SOURCE.md`](assets/ICON_SOURCE.md) for icon sources and licenses.
