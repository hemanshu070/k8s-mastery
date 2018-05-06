## Starting the Web App Locally
` $ yarn start `

## Building the application
` $ yarn build `

## Building the container
` $ docker build -f Dockerfile -t $DOCKER_USER_ID/sentiment-analysis-frontend . `

## Running the container
` $ docker run -d -p 80:80 $DOCKER_USER_ID/sentiment-analysis-frontend `

## Pushing the container
` $ docker push $DOCKER_USER_ID/sentiment-analysis-frontend `

## Pushing to CloudFoundry (with a staticfile buildpack)
`yarn`
`yarn build`
`cf push`


# Pushing to CloudFoundry
Pushing a NodeJS app to CloudFoundry can be done in few different ways. Simplest is to build your app and push it with `staticfile_buildpack`. You can use Pivotal Web Services (http://run.pivotal.io) if you don't have access to your company's CloudFoundry platform.

## Login to CloudFoundry
`cf login -a https://api.run.pivotal.io`

## Build Application
`yarn`
`yarn build`

## Deploy Application
`cf push`

## Notes
There were few changes we needed to do to the original app.
* Add new `manifest.yml` - deployment descriptor that describes how to push to CloudFoundry, e.g. memory requirements, buildpack, start command. 
* Modify `src/App.js` to include the WEB_APP_URL that points to the Spring Boot microservice.

