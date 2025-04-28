# GeoServer on Red Hat JBoss Enterprise Application Platform

<p align="left">
<img src="https://img.shields.io/badge/redhat-CC0000?style=for-the-badge&logo=redhat&logoColor=white" alt="Redhat">
<img src="https://img.shields.io/badge/jboss-eap-CC0000?style=for-the-badge&logo=jboss-eap&logoColor=white" alt="JBossEAP">
<img src="https://img.shields.io/badge/geoserver-%23326ce5.svg?style=for-the-badge&logo=geoserver&logoColor=white" alt="geoserver">
</p>

<p align="left">
  <img src="https://raw.githubusercontent.com/maximilianoPizarro/geoserver-on-jboss-eap/main/geoserver-on-jboss-eap.gif" width="800" title="JBossEAP">
</p>  

# GeoServer

[GeoServer](https://geoserver.org) is an open source software server written in Java that 
allows users to share and edit geospatial data. Designed for interoperability, it publishes data from 
any major spatial data source using open standards.

Being a community-driven project, GeoServer is developed, tested, and supported by a diverse group of 
individuals and organizations from around the world.

GeoServer is the reference implementation of the Open Geospatial Consortium (OGC) 
Web Feature Service (WFS) and Web Coverage Service (WCS) standards, as well as a high performance 
certified compliant Web Map Service (WMS), compliant Catalog Service for the Web (CSW)
and implementing Web Processing Service (WPS). 
GeoServer forms a core component of the Geospatial Web.

[JBoss EAP](https://www.redhat.com/en/technologies/jboss-middleware/application-platform)

## Red Hat JBoss Enterprise Application Platform (JBoss EAP)

Red Hat® JBoss® Enterprise Application Platform (JBoss EAP) delivers enterprise-grade security, performance, and scalability in any environment. Some features of JBoss EAP are:

Optimized for cloud and containers: with simplified deployment and full Jakarta EE performance for applications in any environment.

Lightweight, flexible architecture: which is well-suited for microservices and traditional applications.

Support and standardize microservices development: With the JBoss Enterprise Application Platform expansion pack that supports Eclipse MicroProfile application programming interfaces (APIs).

## JBoss EAP Operator

Operators are software components that extend the Kubernetes API to customize how to manage applications and their components.

The JBoss EAP Operator facilitates the deployment and management of JBoss EAP on container orchestration platforms such as Red Hat OpenShift. The operator acts as an automated management tool, simplifying the process of provisioning, scaling, and updating JBoss EAP applications.

## JBoss EAP Operator capabilities

The JBoss EAP Operator deploys and manages multiple instances of the JBoss EAP Java application in a Red Hat OpenShift cluster.

Simplifies management: Creates, configures, manages, and seamlessly upgrades instances of complex stateful applications. No more manual configuration, making management a breeze.

Efficient scaling: Effortlessly scales the instances of your JBoss EAP Java application based on demand.

Faster updates: Seamlessly updates, enabling you to roll out new operator features and bug fixes without downtime

Safe transaction recovery: Provides safe transaction recovery in your application cluster by verifying all transactions are completed before scaling down the replicas and marking a pod as clean for termination.

## Operator installation and configuration

Let's walk through the steps to install the JBoss EAP Operator on your OpenShift Cluster and deploy your first example application:

### Step 1: Install JBoss EAP from OperatorHub

Click Install in the JBoss EAP operator window.
In the Install Operator window, for Installation mode, select All namespaces on the cluster The operator is available in openshift-operators namespace.

### Step 2: Deploy your application with JBoss EAP Operator

<p align="left">
  <img src="https://github.com/maximilianoPizarro/geoserver-on-jboss-eap/blob/main/geoserver-jboss-eap.PNG?raw=true" width="900" title="Run On Openshift">
</p>

In the main navigation, click the dropdown menu (perspective switcher) and select Developer.

Click Add > Operator Backed > WildFlyServer > Create.

Switch to the YAML view and replace the existing code with the following:

```
apiVersion: wildfly.org/v1alpha1
kind: WildFlyServer
metadata:
  name: geoserver
spec:
  applicationImage: 'docker.osgeo.org/geoserver:2.27.0'
  env:
    - name: GEOSERVER_DATA_DIR
      value: /opt/eap/standalone/data
    - name: CORS_ENABLED
      value: 'false'
    - name: SKIP_DEMO_DATA
      value: 'false'
    - name: JDK_JAVA_OPTIONS
      value: '-Xms1G -Xmx2G -XX:-UseGCOverheadLimit -XX:-ExitOnOutOfMemoryError -XX:+UseParallelGC -Dfile.encoding=UTF8 -Duser.timezone=America/Buenos_Aires'      
    - name: PROXY_BASE_URL
      value: 'http://localhost:8080/geoserver'
  replicas: 1
  serviceAccountName: geoserver-sa
  readinessProbe:
    httpGet:
      path: /geoserver
      port: 8080
  livenessProbe:
    httpGet:
      path: /geoserver
      port: 8080
  storage:
    emptyDir: {}
```
<p align="left">
  <img src="https://github.com/maximilianoPizarro/geoserver-on-jboss-eap/blob/main/geoserver_wilfly.PNG?raw=true" width="900" title="Run On Openshift">
</p>

### Click Create to deploy the application with JBoss EAP Operator.

Log in with admin/geoserver credentials


<p align="left">
  <img src="https://github.com/maximilianoPizarro/geoserver-on-jboss-eap/blob/main/geoserver_running.PNG?raw=true" width="900" title="Run On Openshift">
</p>

<p align="left">
  <img src="https://github.com/maximilianoPizarro/geoserver-on-jboss-eap/blob/main/geoserver_sample.PNG?raw=true" width="900" title="Run On Openshift">
</p>
