Generic Spring Boot chart for applications designed to run on Kubernetes.

Resources created are based on the new-app output and augmented per Perficient specific guidelines:

ImageStream
BuildConfig
DeploymentConfig
Service
Route

By default, the following configuration is assumed:

- web server running on port 8080
- servlet context is /
- actuator health endpoint is on port 8080 at /actuator/health
- actuator metrics endpoint for prometheus is on port 8080 at /actuator/prometheus
- a prometheus service monitor is configured to scrape the /actuator/prometheus endpoint on port "http" 8080 for services labeled with prometheus:spring-boot-service

An ImageStream will be created to reference the provided git repository
A BuildConfig will be created to build the image based on the above ImageStream
The DeploymentConfig will setup the readiness and liveness probe to ping the actuator/health endpoint
A Service will expose port 8080 with a prometheus:spring-boot-service label to enable monitoring
A Route will expose a public URL on port 8080 pointing to the Service

Generating the operator:

mkdir spring-boot
cd spring boot
operator-sdk init --plugins=helm --domain=com.perficient --group=apimc --version=v1alpha1 --kind=SpringBootApp --helm-chart=/path-to-chart
make install
export IMG=quay.io/mrethers/spring-boot-operator:v0.0.14
make docker-build docker-push IMG=$IMG
make deploy IMG=$IMG