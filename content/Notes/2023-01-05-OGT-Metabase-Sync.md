Project: https://github.com/NodeArt/metabase_export_import/tree/fix-mb-v0.48-export-import

## Environment variables:
```sh
MB_DATA_DIR=data

MB_EXPORT_HOST=https://reporting.camisa10.bet/api/
MB_EXPORT_USERNAME=user@nodeart.io
MB_EXPORT_PASSWORD=password
MB_EXPORT_DB=clickhouse

MB_IMPORT_HOST=https://reporting-test.oddsgate.dev/api/
MB_IMPORT_USERNAME=user@nodeart.io
MB_IMPORT_PASSWORD=password
MB_IMPORT_DB=clickhouse-TEST
```

## Running export:
```sh
poetry run python3 metabase_export.py --verbose all
```

## Running import:
We will be running imports as individual commands to separate cards and
dashboards into their individual collections for easier use. They must be run
sequentially the order specified below.
```sh
poetry run python3 metabase_import.py --verbose fields
poetry run python3 metabase_import.py --verbose metrics
poetry run python3 metabase_import.py --verbose cards ogt_cards
poetry run python3 metabase_import.py --verbose dashboards ogt_dashboards
```

Alternatively, to export cards & dashboards into a single collection:
```sh
poetry run python3 metabase_import.py --verbose all ogt_analytics
```

# Notes for deployment

- Separate accounts that are not actively used by any persons are RECOMMENDED to
be created for export & import as mailbox will be spammed with sign-in
notifications each time scripts are running.
- Aforementioned accounts MUST have admin priveleges on their respective
Metabase instances.
- It is RECOMMENDED to user a single email/username/password as it makes
management easier.
- `MB_EXPORT_HOST` and `MB_IMPORT_HOST` MUST have `/api/` as URLs' path.
- `MB_DATA_DIR` will contain all the required data from source once export is
done and copies of it MAY be shared when doing import to multiple targets,
eliminating the need to do multiple exports when doing multiple imports.
- Both export and import are resource-heavy, so it's running them SHOULD be done
when needed rather than doing it as a cronjob.
- Imports MUST be done from a single metabase instance and SHOULD NOT be
actively used for production. All targets MUST only contain imported cards and
dashboards.
- Database structure MUST be identical for source & destination, but database
names MAY be different.
- Collection names(e.g `ogt_cards`, `ogt_dashboards`, `ogt_analytics`) MAY be
different, but they MUST be decided upon beforehand since they SHALL NOT be
shanged afterwards.
- Flags MAY be used instead of environment variables if needed. Running
respective scripts with `--help` command will provide their names.
- Project's dependencies are managed by [Poetry](https://python-poetry.org), but
they MAY be installed in other ways if needed.
- It is RECOMMENDED to use `--verbose` flag for the time being, as it will allow
to more easily identify problems if they arise.
