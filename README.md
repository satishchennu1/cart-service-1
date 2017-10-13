Red Hat Open Shift Microservice Demo  
====================================
This is an example demo showing a retail store consisting of several of microservices based on [JBoss EAP 7](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/) and [Node.js](https://access.redhat.com/documentation/en/openshift-enterprise/3.2/paged/using-images/chapter-1-source-to-image-s2i), deployed to [OpenShift](https://access.redhat.com/products/openshift-enterprise-red-hat/).

It demonstrates how to wire up small microservices into a larger application using microservice architectural principals. The demo is utilizing source to image functionality of Open shift. 

Services
--------
There are several individual microservices and infrastructure components that make up this app:


1. Cart Service - Spring Boot application running on [JBoss EAP 7](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/), manages shopping cart for each customer

![Architecture Screenshot](https://github.com/siamaksade/cart-service/docs/arch-diagram.png "Architecture Diagram")


Prerequisites
================
In order to deploy the CoolStore microservices application, you need an OpenShift environment with
* 4+ GB memory quota if deploying CoolStore
* 16+ GB memory quota if deploying the [complete demo](https://github.com/jbossdemocentral/coolstore-microservice/tree/stable-ocp-3.4/openshift/scripts)
* RHEL and JBoss imagestreams installed (check _Troubleshooting_ section for details)
* Nexus Repository (or other maven repository managers) defined for [JBoss Enterprise Maven Repository](https://access.redhat.com/maven-repository)

Deploy cart Microservices Application
================
Deploy the CoolStore microservices application using this template `openshift/coolstore-template.yaml`:
```
oc new-project cart
oc process -f cart-template.yaml | oc create -f -
```

When all pods are deployed, verify all services are functioning:
```
oc rsh $(oc get pods -o name -l application=coolstore-gw)
curl http://cart:8080/api/cart/FOO
```

Deploy Complete Demo with CI/CD
================
In order to deploy the complete demo infrastructure for demonstrating Microservices, CI/CD, 
agile integrations and more, either order the demo via RHPDS or use the following script to provision the demo
on any OpenShift environment:

```
$ oc login MASTER-URL
$ openshift/scripts/provision-demo.sh 
```

You can delete the demo projects and containers with:
```
$ openshift/scripts/provision-demo.sh --delete
```
 
