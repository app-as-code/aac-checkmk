# Changelog

All notable changes to this deployment definition are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed

- Upstream image tracks the Checkmk Raw `2.4.0-daily` build as defined in `service.yml`.

## [0.0.1]

### Added

- Initial App-as-Code service definition for Checkmk Raw as a single OMD site container.
- Web interface, Livestatus and event console ports with Traefik routing and automatic DNS.
- Persistent site data volume and host time zone mount.
- README with configuration reference and project icon.
