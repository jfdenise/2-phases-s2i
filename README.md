# 2-phases-s2i

The goal is to generate an S2I builder image for a given kind of WildFly server. Such builder is then reused when building application image.

The steps:

* `helm install jaxrs-builder -f helm-jaxrs-builder.yaml wildfly/wildfly`

* `oc import-image quay.io/wildfly/wildfly-runtime-jdk11:latest --confirm` Required currently by WildFly Helm charts. 
The kind applies to both builder and runtime. We are in ccase were the builder is an ImageStreamtag, the runtime a remote docker image.

* `helm install app-image -f helm-application.yaml wildfly/wildfly`

==> You can then access the route of app-image.


