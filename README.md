This repo contains the CHEFS-maintained version of PostgreSQL managed by Patroni for High Availability (HA).

# Build

This image is built as per the OpenShift [templates](./openshift).

## Distribution

This is the old section that describes copying the image to your local namespace.

Run RBAC to create an SA and bind it to, this is done on a lab or build cluster:

```yaml
 kind: ClusterRole
 name: system:image-puller
```

Using the token from the SA above, create a docker registry secret with the appropriate credentials. For the `--docker-server` argument use the **external registry host name**.

```console
oc create secret docker-registry bcgov-tools-klab \
  --docker-server=image-registry.foo.bar.gov.bc.ca \
  --docker-username=bcgov-images-cicd \
  --docker-password=$SATOKEN \
  --docker-email=unused
```

Then allow the builder service account to access the newly minted docker credentials for pulling images:

```console
oc secrets add sa/builder secrets/bcgov-tools-klab --for=pull
```

And finally, create an `imagestreamtag` to import the image to your cluster. Again, for the `-from-image` argument use the **external registry host name**.

```console
oc create imagestreamtag patroni-postgresql:12.4-latest \
  --from-image=image-registry.foo.bar.gov.bc.ca /bcgov-tools/patroni-postgresql:12.4-latest
```

Check to make sure it imported:

```console
oc get is
```

```console
oc describe is/patroni-postgresql
```
