# wara2ak-content

Remote content endpoint for the **???? (Wara2ak)** iOS app. Public on purpose ? the app fetches these files anonymously via raw.githubusercontent.com.

- `version.json` ? tiny manifest the app polls: `{ content_version, schema_version, url }`
- `transactions.vN.json` ? full content payload (SPEC schema 1.x)

## Publishing a content update (no app release needed)

1. Edit a copy of the latest `transactions.vN.json` (fees, docs, gotchas, dependencies).
2. Bump `content_version` (N+1) inside the payload AND save it as `transactions.v{N+1}.json`.
3. Update each changed transaction's `last_verified` to today.
4. Validate: `python3 validate_content.py transactions.v{N+1}.json` (script in the app repo).
5. Update `version.json` to point at the new file with the new version.
6. Commit + push. Every app installs picks it up within ~5 minutes (CDN cache) on next launch.

**Never** ship a fee without re-verifying it on the official portal and updating `last_verified` ? the in-app freshness badge exposes stale data to users by design.
