## Building the Docker Container

```
$ docker build -f Dockerfile -t $DOCKER_USER_ID/sentiment-analysis-logic .
```

## Running the Docker Container

```
$ docker run -d -p 5050:5000 $DOCKER_USER_ID/sentiment-analysis-logic
```

The app is listening by default on port 5000. The 5050 port of the host machine is mapped to the port 5000 of the container.

-p 5050:5000 i.e.

``` -p <hostPort>:<containerPort>```

### Verifying that it works

Execute a POST on endpoint 

-> `localhost:5050/analyse/sentiment` or 

-> `<docker-machine ip>:5050/analyse/sentiment` Docker-machine ip has to be used if your OS doesn't provide native docker support. 

Request body:

```
{
    "sentence": "I hate you!"
}
```

## Pushing to Docker Hub

```
$ docker push $DOCKER_USER_ID/sentiment-analysis-logic
```

# Pushing to CloudFoundry
Pushing a python app to CloudFoundry is extremely simple. You can use Pivotal Web Services (http://run.pivotal.io) if you don't have access to your company's CloudFoundry platform.

## Login to CloudFoundry
`cf login -a https://api.run.pivotal.io`

## Application Deployment
`cf push`

## Notes
There were few changes we needed to do to the original app:
* Add new `manifest.yml` - deployment descriptor that describes how to push to CloudFoundry, e.g. memory requirements, buildpack, start command, etc.
* Add new `sa/nltk.txt` - python buildpack uses nltk to download extra packages, add one-liner `punkt` to this file
* Modify `sa/requirements.txt` - add one-liner `textblob==0.15.0`
* Modify `sa/sentiment_analysis.py` - add `PORT` variable (that the platform will bind the process to, typically 8080)
