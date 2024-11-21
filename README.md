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

# validate the chart with helm's linter, we should get the expected number of charts and no failures

helm lint helm-node-app

# execute a dry run to see if it compiles correctly

helm template helm-node-app

# Pushing to GCR

* services accounts

To create a new service account:


gcloud iam service-accounts create helm-node-app \
--description="Service account for helm-node-app" \
--display-name="helm-node-app"
This creates a service account with the email format:

helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com

then give it the permissions it needs

gcloud projects add-iam-policy-binding <PROJECT_ID> \
--member="serviceAccount:helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com" \
--role="roles/storage.objectViewer"

generate the key file

gcloud iam serv# helm-node-app


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

# validate the chart with helm's linter, we should get the expected number of charts and no failures

helm lint helm-node-app

# execute a dry run to see if it compiles correctly

helm template helm-node-app

# Pushing to GCR

* services accounts

To create a new service account:


gcloud iam service-accounts create helm-node-app \
--description="Service account for helm-node-app" \
--display-name="helm-node-app"
This creates a service account with the email format:

helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com

then give it the permissions it needs

gcloud projects add-iam-policy-binding <PROJECT_ID> \
--member="serviceAccount:helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com" \
--role="roles/storage.objectViewer"

generate the key file

gcloud iam service-accounts keys create key-file.json \
--iam-account=helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com

use the key file to create the secret and add it to the kube context your on

To create a new service account:

bash
Copy code
gcloud iam service-accounts create helm-node-app \
--description="Service account for helm-node-app" \
--display-name="helm-node-app"
This creates a service account with the email format:

kotlin
Copy code
helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com

Authenticate locally with docker (optional)

docker login -u _json_key --password-stdin https://gcr.io < key-file.json

verify permissions by viewing the IAM policy and making sure
you have those 4 compute roles admin, network, etc

gcloud projects get-iam-policy <PROJECT_ID> --flatten="bindings[].members" \
--format="table(bindings.role, bindings.members)" \
--filter="bindings.members:helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com"


test by pulling down the image

docker pull gcr.io/<PROJECT_ID>/<IMAGE_NAME>:<TAG>


* authenticating with GCP


* creating a secret for your docker gcr login
* having the correct permissions for the service account
* creating the secret before you install the helm chart but after you've created the namespace
* making sure that the image is made with same architecture as the cluster, in our case we had arm64 and they had amd64
* making sure you had the correct context for the kubectl, it was connected to CKE it was still on minikube
* you don't have to create load balancer and all of that work online in the console
* running describe on the pod was the best thing because it informs you if the secrets wrong, or if the name or tag is wrong
*
ice-accounts keys create key-file.json \
--iam-account=helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com

use the key file to create the secret and add it to the kube context your on

To create a new service account:

bash
Copy code
gcloud iam service-accounts create helm-node-app \
--description="Service account for helm-node-app" \
--display-name="helm-node-app"
This creates a service account with the email format:

kotlin
Copy code
helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com

Authenticate locally with docker (optional)

docker login -u _json_key --password-stdin https://gcr.io < key-file.json

verify permissions by viewing the IAM policy and making sure
you have those 4 compute roles admin, network, etc

gcloud projects get-iam-policy <PROJECT_ID> --flatten="bindings[].members" \
--format="table(bindings.role, bindings.members)" \
--filter="bindings.members:helm-node-app@<PROJECT_ID>.iam.gserviceaccount.com"


test by pulling down the image

docker pull gcr.io/<PROJECT_ID>/<IMAGE_NAME>:<TAG>


* authenticating with GCP


* creating a secret for your docker gcr login
* having the correct permissions for the service account
* creating the secret before you install the helm chart but after you've created the namespace
* making sure that the image is made with same architecture as the cluster, in our case we had arm64 and they had amd64
* making sure you had the correct context for the kubectl, it was connected to CKE it was still on minikube
* you don't have to create load balancer and all of that work online in the console
* running describe on the pod was the best thing because it informs you if the secrets wrong, or if the name or tag is wrong
* 
