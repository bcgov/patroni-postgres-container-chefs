
# OpenShift Templates

These templates are used to build the Patroni PostgreSQL containers.

## Build

The template will create a `BuildConfig` that is configured to use the given
Patroni and PostgreSQL versions.

```sh
$ oc process -f build.yaml -p PATRONI_VERSION=1.6.5 -p PG_VERSION=12.4 |\
    oc -n a12c97-tools apply -f -
imagestream.image.openshift.io/postgres created
imagestreamtag.image.openshift.io/postgres:12.4 created
imagestream.image.openshift.io/patroni-postgres created
buildconfig.build.openshift.io/patroni-postgres created
```

> Note: PostgreSQL 12 is not longer supported and was the legacy version used in
> this repo.
