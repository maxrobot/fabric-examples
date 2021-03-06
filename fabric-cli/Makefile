#
# Copyright SecureKey Technologies Inc. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0
#

# Supported Targets:
#
# populate: populates all generated files (not included in git)
# fabric-sdk-go-pin: populates only upstream files (not included in git)
# populate-vendor: populates only the vendor directory (not included in git)
# example-network: start the example fabric network
# example-network-clean: stops any running network and cleans up the artifacts
# clean: removes all generated artifacts
#

# Tool commands (overridable)
GO_CMD             ?= go
GO_DEP_CMD         ?= dep
DOCKER_CMD         ?= docker
DOCKER_COMPOSE_CMD ?= docker-compose

# fabric-sdk-go version (overridable)
FABRIC_SDK_GO_BRANCH    ?= master
FABRIC_SDK_GO_COMMIT    ?= 05ffa0a5ae5db0890341fdec498ccc5e684a6ea5

# Test fixture paths
FIXTURE_DOCKERENV_PATH := fabric-sdk-go/test/fixtures/dockerenv

.PHONY: populate
populate: fabric-sdk-go-pin fabric-sdk-go-populate populate-vendor

.PHONY: fabric-sdk-go-pin
fabric-sdk-go-pin:
	@echo "Pinning fabric-sdk-go ..."
	@UPSTREAM_COMMIT=$(FABRIC_SDK_GO_COMMIT) UPSTREAM_BRANCH=$(FABRIC_SDK_GO_BRANCH) scripts/fabric-sdk-go/apply_upstream.sh

.PHONY: fabric-sdk-go-populate
fabric-sdk-go-populate:
	@scripts/fabric-sdk-go/populate.sh

.PHONY: populate-vendor
populate-vendor:
	@echo "Populating vendor ..."
	@$(GO_DEP_CMD) ensure -vendor-only

.PHONY: clean
clean: example-network-clean
	rm -Rf vendor
	rm -Rf fabric-sdk-go

.PHONY: example-network
example-network:
	@cd ./fabric-sdk-go && \
		make dockerenv-stable-up

.PHONY: example-network-clean
example-network-clean:
	@DOCKER_CMD=$(DOCKER_CMD) scripts/fabric-sdk-go/example-network-clean.sh
