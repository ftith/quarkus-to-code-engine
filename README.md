# quarkus-to-code-engine
Deployment of a RESTEasy Quarkus application to IBM Cloud Engine

## Prerequisites
- JDK 11+ any distribution
- docker
- (optional) GraalVM
- (optioonal) Maven or Gradle
- ibmcloud CLI

## Demo overview
![livecoding](https://user-images.githubusercontent.com/27813737/132954389-b592784f-a06a-46f4-9fcc-c319f2f5b290.png)

## Steps
1. Go to https://quarkus.io
2. Download from https://code.quarkus.io
- Tick `RESTEasy JAX-RS`
- Click on Generate your application
3. Open the project with your favorite IDE
4. `mvn compile quarkus:dev`
5. Access http://localhost:8080
6. `mvn package -Pnative -Dquarkus.native.container-build=true`
7. `docker build -f src/main/docker/Dockerfile.native -t <repo>/hello-resteasy .`
8. `docker push demo/hello-resteasy`
9. `ibmcloud login`
10. `ibmcloud target -g <group_name>`
11. Optionally, install code engine plugin: `ibmcloud plugin install code-engine -f`
13. `ibmcloud ce project select --name <project_name>`
14. `ibmcloud ce app list`
15. `ibmcloud ce application create --name myapp --image <repo>/hello-resteasy`
