IMAGE_ROOT?=localhost
IMAGE_NAME=rocm_acpp
IMAGE_VERSION=0.1.0

LLVM_VERSION=18
ACPP_VERSION=24.06.0

SIF_FILE=$(IMAGE_NAME)_$(IMAGE_VERSION).sif
TAR_FILE=$(IMAGE_NAME)_$(IMAGE_VERSION).tar

build: Dockerfile
	docker build \
		-t $(IMAGE_ROOT)/$(IMAGE_NAME):$(IMAGE_VERSION) \
		--build-arg LLVM_VERSION=${LLVM_VERSION} \
		--build-arg ACPP_VERSION=${ACPP_VERSION} \
		.

push:
	docker push $(IMAGE_ROOT)/$(IMAGE_NAME):$(IMAGE_VERSION)

singularity:
	rm -f $(SIF_FILE) $(TAR_FILE)
	docker save $(IMAGE_ROOT)/$(IMAGE_NAME):$(IMAGE_VERSION) -o $(TAR_FILE)
	singularity build $(SIF_FILE) docker-archive://$(TAR_FILE)
	rm -f $(TAR_FILE)
