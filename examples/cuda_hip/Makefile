IMAGE_ROOT?=localhost
IMAGE_NAME=cuda_hip
IMAGE_VERSION=0.1.0

SIF_FILE=$(IMAGE_NAME)_$(IMAGE_VERSION).sif
TAR_FILE=$(IMAGE_NAME)_$(IMAGE_VERSION).tar


build: Dockerfile
	docker buildx build --platform linux/amd64 \
		-t $(IMAGE_ROOT)/$(IMAGE_NAME):$(IMAGE_VERSION) \
		--build-arg CUDA_VERSION=11.5.0 \
		--build-arg HIP_VERSION=5.3.3 \
		.

push:
	docker push $(IMAGE_ROOT)/$(IMAGE_NAME):$(IMAGE_VERSION)

singularity:
	rm -f $(SIF_FILE) $(TAR_FILE)
	docker save $(IMAGE_ROOT)/$(IMAGE_NAME):$(IMAGE_VERSION) -o $(TAR_FILE)
	singularity build $(SIF_FILE) docker-archive://$(TAR_FILE)
	rm -f $(TAR_FILE)
