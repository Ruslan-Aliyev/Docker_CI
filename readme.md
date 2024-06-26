# GitHub to DockerHub

- Method 1 Tutorial: https://github.com/tonysm/dockerforlaravel-app/blob/master/.github/workflows/docker-images.yml
  - ( https://github.com/elgohr/Publish-Docker-Github-Action )
- Method 2 Tutorial: https://docs.github.com/en/actions/guides/publishing-docker-images#publishing-images-to-docker-hub

Setup Secrets

![](/Illustrations/secrets.png)

That's the same username and password for logging into your DockerHub

## Method 1

```
name: Build Docker Images

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Publish to Registry
      uses: elgohr/Publish-Docker-Github-Action@master
      with:
          name: atabegruslan/docker-ci-test
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
```

![](/Illustrations/method_1.png)

## Method 2

```
name: Publish Docker image

on:
  push:
    branches:
      - master

jobs:
  push_to_registry:
    name: Push Docker image to Docker Hub
    runs-on: ubuntu-latest
    steps:
      - name: Check out the repo
        uses: actions/checkout@v2
      
      - name: Log in to Docker Hub
        uses: docker/login-action@f054a8b539a109f9f41c372932f1ae047eff08c9
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      
      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@98669ae865ea3cffbcbaa878cf57c20bbf1c6c38
        with:
          images: atabegruslan/docker-ci-test
      
      - name: Build and push Docker image
        uses: docker/build-push-action@ad44023a93711e3deb337508980b4b5e9bcdc5dc
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

![](/Illustrations/method_2.png)

![](/Illustrations/dockerhub.png)
