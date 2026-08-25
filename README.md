# OfficeRiders Status

Public status page for OfficeRiders services, forked from
[Kicoba Status Page](https://github.com/Kicoba/kicoba-status-page).

The site is static and served by GitHub Pages so it remains available during an
OfficeRiders or GCP outage. GitHub Actions checks the public surfaces every five
minutes. Detected outages create GitHub issues, and recovery closes them
automatically. Those issues provide the 90-day uptime and incident history shown
on the page.

Public URL: <https://status.officeriders.com>

## Monitored surfaces

The public page reports these customer-facing components:

- Public website: `PROBE_URL_LANDING`
- Web app: `PROBE_URL_APP`
- Host app: `PROBE_URL_HOST`
- API: `PROBE_URL_API`

The URLs are stored as GitHub repository variables. They are not embedded in the
source configuration.

Private infrastructure probes run in the same monitor workflow but never create
public incidents or print their URLs:

- Argos control plane: `PROBE_URL_PRIVATE_ARGOS`
- Vault: `PROBE_URL_PRIVATE_VAULT`

Private probe URLs are stored as GitHub repository secrets. A private failure
fails the monitor workflow for the infrastructure team without adding the
internal endpoint to the public status page.

## Incident model

The monitor opens at most one issue for each affected component. Automated
issues use `component:<id>` and `severity:<severity>` labels. When the component
recovers, the monitor adds a recovery comment and closes the issue.

Manual incidents can use the same labels and are included in the public history.
Do not include secrets, customer data, internal hostnames, tokens, logs, or raw
environment values in an issue.

## Configuration

Component definitions and probe mappings live in `monitor.config.json`. Probe
URLs are provided only through GitHub variables or secrets referenced by their
`urlEnv` names.

Required repository settings:

- Actions workflow permissions allow GitHub Actions to create issues.
- GitHub Pages uses GitHub Actions as its build and deployment source.
- Custom domain is `status.officeriders.com`.
- DNS `CNAME status.officeriders.com -> work-lib.github.io` is DNS-only.

## Local verification

```sh
npm ci
npm run build
```

## Security and compliance evidence

- Git history records monitoring configuration changes.
- GitHub Actions retains monitor and deployment execution evidence.
- GitHub issues retain incident opening, recovery, and closure timestamps.
- Public leakage checks run in CI and before each Pages deployment.
- Private probe URLs remain in GitHub Actions secrets.

This evidence supports incident response, monitoring, availability, and change
management controls. Control owners remain responsible for retention settings,
access reviews, alert ownership, and periodic recovery tests.

## Upstream and license

This repository is a real GitHub fork of
[`Kicoba/kicoba-status-page`](https://github.com/Kicoba/kicoba-status-page). Keep
the `upstream` remote when working locally so improvements can be synchronized.

Licensed under the European Union Public Licence v1.2. See [`LICENSE`](LICENSE).
The footer retains the Kicoba attribution requested by the upstream project.
