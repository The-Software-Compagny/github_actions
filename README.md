# Github Actions

Simple Github actions to implement on your NodeJS projects, etc.

## Available actions
### Docker-image
```yml
name: Docker Image CI

on:
  release:
    types: [published]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout the code
        uses: actions/checkout@v2

      - name: Build docker
        uses: StreamKITS/github_actions/docker-image@main
        with:
          context: .
          repository: ${{ github.repository }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
```

### Release
```yml
name: Release

on:
  workflow_dispatch:
    inputs:
      version_increment:
        description: 'La version a incrémenter (major, minor, patch)'
        required: true
        default: 'patch'
        type: choice
        options:
        - 'major'
        - 'minor'
        - 'patch'
      build_docker_image:
        description: "Construire l'image docker ?"
        required: true
        default: true
        type: boolean
      latest:
        description: "Tagger l'image docker avec le tag 'latest' ?"
        required: true
        default: true
        type: boolean

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Build docker
        uses: StreamKITS/github_actions/release@main
        with:
          version_increment: ${{ github.event.inputs.version_increment }}
          build_docker_image: ${{ github.event.inputs.build_docker_image }}
          latest: ${{ github.event.inputs.latest }}
          repository: ${{ github.repository }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          # Optional parameters, thoses are default values :
          registry: 'ghcr.io'
          context: .
          build-args: ''
```
Badge de release :
```md
[![Release](https://github.com/<repo_owner>/<repo_name>/actions/workflows/release.yml/badge.svg)](https://github.com/<repo_owner>/<repo_name>/actions/workflows/release.yml?event=workflow_dispatch)
```

### K8s-Deploy
Soon...
