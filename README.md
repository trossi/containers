# Container examples

## Building a new image

Follow these instructions if you need to update the container image based on `Dockerfile`.

**First:** Update the image version number in `Makefile`.

Then, build and push the new image on a local machine following the instructions below.

### Ubuntu

If you don't have docker or podman, install using

    sudo apt install podman-docker

If using podman, define

    export BUILDAH_FORMAT=docker

Build the image

    make build

Optional: Convert the local image to singularity:

    make singularity

Optional: Push the image:

    docker login ghcr.io
    make push

Login to ghcr.io using GitHub Personal Access Token with scope 'write:packages'.
See [these instructions](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic).


## Using built image via docker/podman

Run python with the image:

    docker run -it --rm --entrypoint python3 IMAGE_NAME:IMAGE_VERSION


## Using built image via singularity/apptainer

Use the locally created sif file or pull the pushed image:

    export SINGULARITY_DOCKER_USERNAME=...
    export SINGULARITY_DOCKER_PASSWORD=...
    singularity pull docker://ghcr.io/NAMESPACE/IMAGE_NAME:IMAGE_VERSION

Use GitHub Personal Access Token with scope 'read:packages' to pull private images.
See [these instructions](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic).

Run python with the image:

    singularity exec FILE.sif python3
