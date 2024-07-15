## ---------- UTILS
.PHONY: help
help: ## Show this menu
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)



## ---------- MAIN
.PHONY: lint
lint: ## Run a code linter
	docker run --rm \
		-v ${PWD}:/scan \
		-w /scan \
		hadolint/hadolint:2.12.0-alpine \
			hadolint -c cfg/hadolint.yaml  Dockerfile

.PHONY: scan
scan-files: ## Run a static code analysis
	docker run --rm \
		-v ${PWD}:/scan \
		-w /scan \
		checkmarx/kics:v2.1.1-alpine scan \
			--config cfg/kics.yaml

.PHONY: scan
scan-image: ## Run a security scanner
	[ ! -d /tmp/trivy ] && mkdir /tmp/trivy || true
	docker run \
		-v /run/containerd/containerd.sock:/run/containerd/containerd.sock \
		-v /var/run/docker.sock:/var/run/docker.sock \
		-v /tmp/trivy:/tmp/trivy \
		aquasec/trivy:0.53.0 image \
			--cache-dir=/tmp/trivy \
			--format='table' \
			--severity='UNKNOWN,LOW,MEDIUM,HIGH,CRITICAL' \
			--ignore-unfixed=true \
			--exit-code=1 \
			aleroxac/myapp:v1

.PHONY: build
build: ## Build the container image
	@docker build --build-arg ENV=dev -t aleroxac/myapp:v1 -f Dockerfile .

.PHONY: push
push: ## Push the image to the container registry
	@docker push aleroxac/myapp:v1

.PHONY: run
push: ## Run the container
	@docker run -d --name=myapp -p 8080:8080 --env-file=${PWD}/.env aleroxac/myapp:v1
