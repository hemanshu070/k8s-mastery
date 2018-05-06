## Packaging the application
` $ ./mvnw install`

## Running the application
` $ java -jar sentiment-analysis-web-0.0.1-SNAPSHOT.jar --sa.logic.api.url=http://localhost:5000 ` 

## Building the container
` $ docker build -f Dockerfile -t $DOCKER_USER_ID/sentiment-analysis-web-app . `

## Running the container
``` 
$ docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='http://<container_ip or docker machine ip>:5000' $DOCKER_USER_ID/sentiment-analysis-web-app  
```

#### Native docker support needs the Container IP
CONTAINER_IP: To forward messages to the sa-logic container we need to get  its IP. To do so execute:

` $ docker container list`

Copy the id of sa-logic container and execute:

` $ docker inspect <container_id> `

The Containers IP address is found under the property NetworkSettings.IPAddress, use it in the RUN command.

#### Docker Machine on a VM 
Get Docker Machine IP by executing:

` $ docker-machine ip `

Use this one in the command.


## Pushing the container
` $ docker push $DOCKER_USER_ID/sentiment-analysis-web-app `


# Pushing to CloudFoundry
Pushing a Spring Boot app to CloudFoundry is extremely simple. You can use Pivotal Web Services (http://run.pivotal.io) if you don't have access to your company's CloudFoundry platform.

## Login to CloudFoundry
`cf login -a https://api.run.pivotal.io`

## Build Application
`./mvnw clean install`

## Deploy Application
`cf push`

## Notes
There were only few changes we needed to do to the original app, as Spring Boot apps naturally run really well on CloudFoundry.
* Add new `manifest.yml` - deployment descriptor that describes how to push to CloudFoundry, e.g. memory requirements, buildpack. We also added environment variable `sa.logic.api.url` to `manifest.yml`. Please update with the URL where you deployed `sa-logic` python microservice.
* We also added `.mvn` for Maven wrapper JAR files, so you can build the Java project by executing `mvnw`. Typical Spring Boot application will provide `.mvn/`, `mvnw` and `mvnw.cmd` for easy project building.
* We also had to change the executable bit on `mvnw` and `mvwn.cmd` since that wasn't checked in properly.
* Other than that, Spring Boot apps can run out-of-the-box on CloudFoundry.

