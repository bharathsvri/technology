# PWA and `@angular/service-worker`

## Progressive Web App

Installable web app: **offline cache**, **background sync** (limited by browser), **push** notifications (with permission).

## `@angular/service-worker`

Production `ng build` with service worker enabled generates **ngsw** config (`ngsw-config.json`) for asset/data groups, versioning, and safe updates.

## Update flow

`SwUpdate` service emits when new version available; prompt user to **activateUpdate()** and reload.

## Caching strategies

**Prefetch**, **lazy cache**, **freshness** — tune per API vs static assets.

## HTTPS

Service workers require **secure origin** (localhost exempt in dev).

Next: [Security — XSS and sanitization](35-Security-XSS-and-DomSanitizer.md).
