To build an image from a Dockerfile using GitHub Actions, you can use the docker/build-push-action. This action allows you to build Docker images directly from your repository and optionally push them to a container registry.

Here’s a step-by-step guide to set up a GitHub Actions workflow for building an image from a Dockerfile:

Example Workflow
Create a .github/workflows/docker-build.yml file in your repository with the following content:

name: Build Docker Image

on:
  push:
    branches:
      - main  # Trigger the workflow on pushes to the main branch
  pull_request:  # Trigger the workflow on pull requests

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout the repository
      - name: Checkout code
        uses: actions/checkout@v4

      # Step 2: Set up Docker Buildx
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      # Step 3: Build the Docker image
      - name: Build Docker image
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Dockerfile
          tags: my-docker-image:latest
Explanation of the Workflow
Trigger Events:

The workflow triggers on push events to the main branch and on pull_request events.
Checkout Code:

The actions/checkout action checks out your repository so that the Dockerfile and other necessary files are available for the build.
Set up Docker Buildx:

The docker/setup-buildx-action sets up Docker Buildx, which is required for advanced Docker builds.
Build Docker Image:

The docker/build-push-action builds the Docker image using the specified context (the directory containing the Dockerfile) and file (the path to the Dockerfile).
The tags option specifies the name and tag of the resulting Docker image (e.g., my-docker-image:latest).
Advanced Options
1. Push to a Container Registry

If you want to push the built image to a container registry (e.g., Docker Hub, GitHub Container Registry), you can add a login step and configure the push option:

      # Step 4: Log in to Docker Hub
      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      # Step 5: Build and push the Docker image
      - name: Build and push Docker image
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: my-dockerhub-username/my-docker-image:latest
Make sure to add your Docker Hub credentials (DOCKER_USERNAME and DOCKER_PASSWORD) as GitHub Secrets⁠.

2. Multi-Platform Build

To build a multi-platform image (e.g., for linux/amd64 and linux/arm64), you can specify the platforms option:

      - name: Build and push multi-platform Docker image
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Dockerfile
          push: true
          platforms: linux/amd64,linux/arm64
          tags: my-dockerhub-username/my-docker-image:latest
3. Use Build Arguments

If your Dockerfile uses build arguments, you can pass them using the build-args option:

      - name: Build Docker image with build arguments
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Dockerfile
          build-args: |
            ARG_NAME1=value1
            ARG_NAME2=value2
          tags: my-docker-image:latest
Documentation
For more advanced configurations, refer to the official documentation:

Docker Build-Push Action⁠
GitHub Actions Documentation⁠
Let me know if you need further assistance!