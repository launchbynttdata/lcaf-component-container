SHELL := /bin/bash

CONFTEST ?= conftest
POLICY_COMPONENT_PATHS ?= components/policy/lib components/policy/policy components/policy/waivers

.PHONY: lint
lint::
	$(CONFTEST) test --all-namespaces Dockerfile* --policy components/container/policy $(POLICY_COMPONENT_PATHS)
