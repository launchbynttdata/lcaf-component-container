SHELL := /bin/bash

CONFTEST ?= conftest

.PHONY: lint
lint::
	$(CONFTEST) test --all-namespaces Dockerfile* --policy components/container/policy components/policy/*

