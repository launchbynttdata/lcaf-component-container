SHELL := /bin/bash

DOCKER ?= docker
DOCKER_COMPOSE ?= docker compose
DOCKER_BUILD_ARCH ?= linux/arm64
CONFTEST ?= conftest
POLICY_COMPONENT_PATHS ?= components/policy/lib components/policy/policy components/policy/waivers
SOURCE_FOLDER ?= source

.PHONY: lint
lint::
	$(CONFTEST) test --all-namespaces Dockerfile* --policy components/container/policy $(POLICY_COMPONENT_PATHS)

# The target that checks if the required environment variables are set
.PHONY: docker/check-env-vars
docker/check-env-vars:
ifeq ($(CONTAINER_REGISTRY),)
    $(error CONTAINER_REGISTRY is not set. Please set it before running make.)
endif
ifeq ($(CONTAINER_IMAGE_NAME),)
    $(error CONTAINER_IMAGE_NAME is not set. Please set it before running make.)
endif
ifeq ($(CONTAINER_IMAGE_VERSION),)
    $(error CONTAINER_IMAGE_VERSION is not set. Please set it before running make.)
endif

.PHONY: build
build : docker/check-env-vars
	@echo "Building with CONTAINER_REGISTRY=$(CONTAINER_REGISTRY), CONTAINER_IMAGE_NAME=$(CONTAINER_IMAGE_NAME), CONTAINER_IMAGE_VERSION=$(CONTAINER_IMAGE_VERSION)"
	$(DOCKER) buildx build --platform $(DOCKER_BUILD_ARCH) $(BUILD_ARGS) -t $(CONTAINER_REGISTRY)/$(CONTAINER_IMAGE_NAME):$(CONTAINER_IMAGE_VERSION) --load $(SOURCE_FOLDER)/.

.PHONY: docker_compose/start
docker_compose/start:
	@if [ -f 'docker-compose.yaml' ] || [ -f 'docker-compose.yml' ]; then \
		$(DOCKER_COMPOSE) up -d --remove-orphans; \
	fi

.PHONY: docker_compose/stop
docker_compose/stop:
	@if [ -f 'docker-compose.yaml' ] || [ -f 'docker-compose.yaml' ]; then \
		$(DOCKER_COMPOSE) down; \
	fi

.PHONY: docker/push
docker/push : docker/build
	$(DOCKER) buildx build --platform $(DOCKER_BUILD_ARCH) $(BUILD_ARGS) -t $(CONTAINER_REGISTRY)/$(CONTAINER_IMAGE_NAME):$(CONTAINER_IMAGE_VERSION) --push $(SOURCE_FOLDER)/.

.PHONY: docker/aws_ecr_login
docker/aws_ecr_login:
	aws ecr get-login-password | $(DOCKER) login --username AWS --password-stdin $(CONTAINER_REGISTRY)
