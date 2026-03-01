# spec_i_data

OTA database release repository for the **Spec-I** app.

## How it works

Each GitHub Release contains a single asset — `spec_i.db` — which is a
pre-built SQLite database of CPU (and later GPU/SoC) specifications.

The Spec-I app checks this repository's `/releases/latest` endpoint on
startup and downloads the database if the remote version is newer than the
locally installed one, with no app update required.

## Release versioning

Releases are tagged `v1`, `v2`, `v3`, … (monotonically incrementing integers).
The version number is embedded in the `db_metadata` table inside `spec_i.db`:

```sql
SELECT value FROM db_metadata WHERE key = 'version';
```

## Automated builds

A GitHub Actions workflow (`.github/workflows/build_db.yml`) runs every
**Sunday at 00:00 UTC** (and can also be triggered manually) to:

1. Download the latest processor datasets from:
   - Intel: [toUpperCase78/intel-processors](https://github.com/toUpperCase78/intel-processors)
   - AMD + Benchmarks: [felixsteinke/cpu-spec-dataset](https://github.com/felixsteinke/cpu-spec-dataset)
2. Run the Python ETL script from the [KBitWare/spec_i](https://github.com/KBitWare/spec_i) app repo
3. Verify the output database is non-empty
4. Publish a new GitHub Release with `spec_i.db` attached

## Manual release

To publish a release manually:

1. Go to **Actions → Build & Release Database → Run workflow**
2. The workflow will build, verify, and publish automatically

## Related

- App repository: [KBitWare/spec_i](https://github.com/KBitWare/spec_i)
