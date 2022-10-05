# 2-phases-s2i

The goal is to generate an S2I builder image for a given kind of WildFly server. Such builder is then reused when building application image.

The steps:

* `helm install jaxrs-builder -f helm-jaxrs-builder.yaml wildfly/wildfly`

* `oc import-image quay.io/wildfly/wildfly-runtime-jdk11:latest --confirm` Required currently by WildFly Helm charts. 
The kind applies to both builder and runtime. We are in ccase were the builder is an ImageStreamtag, the runtime a remote docker image.

* `helm install app-image -f helm-application.yaml wildfly/wildfly`

==> You can then access the route of app-image.

Alternative, s2i Binary build
That is another way to deploy the application on openshift

The steps:

* `helm install jaxrs-builder -f helm-jaxrs-builder.yaml wildfly/wildfly` [If not already done in previous steps]

* Build the application locally: `mvn clean package -Popenshift`

* `oc new-build --strategy source --binary --image-stream jaxrs-builder-build-artifacts --name twophase-binary-build`

* `oc start-build twophase-binary-build --from-file target/ROOT.war`

* `oc new-app twophase-binary-build`

* `oc expose svc/twophase-binary-build`

==> You can then access the route of twophase-binary-build.
