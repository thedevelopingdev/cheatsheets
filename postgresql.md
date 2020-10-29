# PostgreSQL cheatsheet

## General information

|key|value|
|:--|:---:|
|Data storage location|`/var/lib/postgresql/data`|
|Default port|5432|

## Docker
### Environment variables
|Name|Required?|Default Value|Description|
|:---|:-------:|:-----------:|:----------|
|`POSTGRES_PASSWORD`|required||Sets the superuser password for Postgres|
|`POSTGRES_USER`|optional|`postgres`|Sets the superuser username for Postgres|
|`POSTGRES_DB`|optional|`${POSTGRES_USER}`|Sets the name of the default database when the image is first started|
