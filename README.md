# Camel Request/Reply Worker

## Requirements

- [Apache Maven 3.x](http://maven.apache.org)

## Preparing

Build the project source code

```
$ cd $PROJECT_ROOT
$ mvn clean install
```

## Running the example standalone

```
$ cd $PROJECT_ROOT
$ mvn spring-boot:run
```

## Running the example in OpenShift

It is assumed that:

- OpenShift platform is already running, if not you can find details how to [Install OpenShift at your site](https://docs.openshift.com/container-platform/3.9/install_config/index.html).
- Your system is configured for Fabric8 Maven Workflow, if not you can find details in the [Getting Started Guide](https://access.redhat.com/documentation/en-us/red_hat_fuse/7.0/html/fuse_on_openshift_guide/)

Create a new project:

```
$ oc new-project demo-fuse
```

Create the [ServiceAccount](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/):

```
$ oc create -f src/main/kube/serviceaccount.yml
```

Create the [ConfigMap](https://kubernetes.io/docs/user-guide/configmap/):

```
$ oc create -f src/main/kube/configmap.yml
```

Create the [Secret](https://kubernetes.io/docs/concepts/configuration/secret/):

```
$ oc create -f src/main/kube/secret.yml
```

Add the [Secret](https://kubernetes.io/docs/concepts/configuration/secret/) to the [ServiceAccount](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/) created earlier:

```
$ oc secrets add sa/camel-request-reply-worker-sa secret/camel-request-reply-worker-secret
```

Add the view role to the [ServiceAccount](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/):

```
$ oc policy add-role-to-user view system:serviceaccount:camel-request-reply-worker:camel-request-reply-worker-sa
```

The example can be built and run on OpenShift using the following goals:

```
$ mvn clean install fabric8:resource fabric8:build fabric8:deploy
```

## Testing the code

Use the same tools/instructions for testing standalone (above). Make sure to change the host/port to point to the exposed OpenShift Route.
