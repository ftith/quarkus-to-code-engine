# quarkus-to-code-engine
Deployment of a RESTEasy Quarkus application to IBM Cloud Engine

## Prerequisites
- JDK 11+ any distribution
- docker
- (optional) GraalVM
- (optional) Maven or Gradle
- ibmcloud CLI
- (optional) Hey

## Demo overview
![livecoding](https://user-images.githubusercontent.com/27813737/132954389-b592784f-a06a-46f4-9fcc-c319f2f5b290.png)

## Steps

### Download the project template
1. Go to https://quarkus.io
2. Download from https://code.quarkus.io
- Tick `RESTEasy JAX-RS`
- Click on Generate your application

### Running the application in dev mode

You can run your application in dev mode that enables live coding using:

`mvn compile quarkus:dev`


Access your application at http://localhost:8080

### Packaging and running the application

The application can be packaged using:

`mvn package`


The application is now runnable using:

`java -jar target/quarkus-app/quarkus-run.jar`



You can create a native executable using:

`mvn package -Pnative`


You can then execute your native executable with: 

`./target/hello-resteasy-1.0.0-SNAPSHOT-runner`



Or, if you don't have GraalVM installed, you can run the native executable build in a container using:

`mvn package -Pnative -Dquarkus.native.container-build=true`


If you want to learn more about building native executables, please consult https://quarkus.io/guides/maven-tooling.html.

### Build and push docker container to dockerhub
Build docker container using:

`docker build -f src/main/docker/Dockerfile.native -t <repo>/hello-resteasy .`

Login to dockerhub

`docker login`

Push docker image using:

`docker push demo/hello-resteasy`

### Push to IBM Cloud Code Engine
Create a free IBM Cloud account using the following link: https://ibm.biz/l_heure_du_dev
Login to ibm cloud using the CLI and target a specific group

```
ibmcloud login
ibmcloud target -g <group_name>
```

Optionally if it is not installed, install code engine plugin: 

`ibmcloud plugin install code-engine -f`

Use the UI to create a project then select it via CLI:

`ibmcloud ce project select --name <project_name>`

Create the application using the docker image that you have pushed to dockerhub:

`ibmcloud ce application create --name <app_name> --image <repo>/hello-resteasy`

Check the list of app:

`ibmcloud ce app list`

### Test the autoscaling
`hey -n <nb_requests_to_run> -c <nb_workers_to_run_concurrently> https://<url_of_app>/hello`
