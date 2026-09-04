# APPS.md — the app-package registry

`BASE-DESIGN.md` §11.3's org-level catalogue of every reusable app package in this ecosystem —
what already exists, at what version, with what required host settings. Paste this file into an
agent's context at the start of a new project; it turns "build a media-cleanup system" into
"install the one that already exists."

Update this table as the **last step of every app's release workflow** — right after tagging,
before considering the release done. A stale entry here is worse than no entry: it tells the next
agent something is safe to install at a version that no longer exists.

| App | Latest | Purpose | Notes |
|---|---|---|---|
| `appkit` | v2.0.1 | Shared cache/mixin/error-envelope helpers + the frontend `HttpClient`/provider | Every other app depends on this — not itself an installable feature. PyPI: `hjtdev-appkit`. npm: `@hjtdev/appkit`. |
| `hjtdev-django-cleanup` | v1.0.1 | Orphaned-media scanning, Jazzmin review page, full cleanup-run history, auto-hooks every `FileField`/`ImageField` via upstream `django-cleanup` | Importable module is `cleanup_app`, never `django_cleanup` (upstream owns that name). Extra: `[celery]` — the app is fully functional with no worker running. npm: `@hjtdev/django-cleanup`. Requires zero `.env` keys. No user-facing surface — every endpoint is admin-only. |
| `django-dynamic-user` | v1.0.0 | Swappable `User`/`Profile`/`Setting` data layer — self-service + admin DRF surfaces, Jazzmin admin, opt-out account-deletion flow, mixin library (`Avatar`, `Timestamp`, `History`, `SoftDelete`, `Verification`, `LastSeen`, `Metadata`) | Importable module is `dynamic_user`. npm: `@hjtdev/django-dynamic-user`. Does no authentication itself — reaches the active user only via `get_user_model()`, same as `django.contrib.auth`. Supplies `AUTH_USER_MODEL` (and `DYNAMIC_USER_PROFILE_MODEL`/`DYNAMIC_USER_SETTING_MODEL`) — a pre-first-migrate host decision, not a runtime setting. Extras: `[celery]` (scheduled deletion finalize/purge — the app is fully functional with no worker running), `[avatar]` (Pillow, for `AvatarMixin`). Requires zero `.env` keys. Registers two frontend basePaths (`dynamic_user`, `dynamic_user_admin`) — both are needed on `@hjtdev/appkit`'s `ApiClientProvider`. |

## Adding an app to this table

One row, in the same shape as the two above:

- **App** — the PyPI/npm distribution name (or the shorter conventional name if there's no
  ambiguity, as with `appkit`).
- **Latest** — the tag, not the version string alone (`v1.0.1`, not `1.0.1`) — matches how a host
  pins it.
- **Purpose** — one line, specific enough that "do I need this?" is answerable without opening the
  repo.
- **Notes** — anything a host must know before installing: the importable module name if it
  differs from the distribution name, required extras, a real "Host action" from the changelog
  that's still relevant, anything that would otherwise cost a re-read of the full README.
