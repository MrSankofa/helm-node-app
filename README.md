# helm-node-app


1. Download Docker Desktop
2. ensure docker is installed correctly with

```docker --version ```

3. ensure that the ~/.docker/config.json is correct

remove the credsStore for our purposes or use "cresStore": "osxkeychain"

Note if you see the Auths section you are logged

```JSON
{
  "auths": {
    "https://index.docker.io/v1/": {
      "serveraddress": "https://index.docker.io/v1/"
    }
  },
  "currentContext": "desktop-linux",
  "plugins": {
    "-x-cli-hints": {
      "enabled": "true"
    },
    "debug": {
      "hooks": "exec"
    },
    "scout": {
      "hooks": "pull,buildx build"
    }
  },
  "features": {
    "hooks": "true"
  }
}
```

4. docker login -u 
  this works better than just docker login

5. create the dockerfile trying to reproduce these steps as necessary for the dev apps you creat

# Determine the runtime as a base image, example node:18
# Set the working directory inside the container
# copy the list of dependencies for the app and install them e.g. package.json or pom.xml npm install or mvn clean install
# Copy application code i.e. the complied code gathered before the docker image is built. and specify the destination of where it will live
# expose any ports was the application is ran
# outline how you start the application, e.g. for node you use node and the filename index.js

6. After creating the dockerfile, we need to build the image
7. confirm that it works by running the image
8. tag the image
9. push the image to the docker repository to be used elsewhere
10. Helmify the app so that we can orchestrate the full stack app, create production, development and POC environments
11. confirm that the app was helmified correctly by

# checking the directory for these files

helm-node-app/
├── charts/          # Directory to include dependent charts
├── templates/       # Kubernetes manifest templates
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── _helpers.tpl # Template helpers
│   └── ...          # Other default template files
├── values.yaml      # Default configuration values for your chart
├── Chart.yaml       # Metadata about the chart (name, version, etc.)
└── .helmignore      # Files to ignore when packaging the chart


# inspect the chart metadata, for the first version it should look like this

```yaml
apiVersion: v2
name: helm-node-app
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: "1.16.0"
```

# validate teh chart with helm's linter, we should get the expected number of charts and no failures

helm lint helm-node-app

# execute a dry run to see if it compiles correctly

helm template helm-node-app
