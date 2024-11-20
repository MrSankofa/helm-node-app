# helm-node-app


1. Download Docker Desktop
2. ensure docker is installed correctly with

```docker --version ```

3. ensure that the ~/.docker/config.json is correct

remove the credsStore for our purposes or use "cresStore": "osxkeychain"

Note if you see the Auths section you are logged in

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