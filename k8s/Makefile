## ---------- UTILS
.PHONY: help
help: ## Show this menu
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)



## ---------- KIND
.PHONY: cluster_up
cluster_up: ## Put the kind cluster up
	@kind create cluster --name kind
	@kubectl cluster-info --context kind
	@kubectl config set-context kind
	@kubectl create namespace dev
	@kubectl config set-context kind --namespace dev

.PHONY: cluster_down
cluster_down: ## Put the kind cluster down
	@kind delete cluster --name kind



## ---------- MAIN
.PHONY: build
build: ## Build the container image
	@docker build -t aleroxac/myapp:v1 .

.PHONY: push
push: ## Push the image to the container registry
	@docker push aleroxac/myapp:v1

.PHONY: deploy
deploy: ## Deploy app on kubernetes
	@sed 's/IMAGE_TAG/v1/g' k8s/app.yaml | kubectl apply -f-
	@kubectl wait --timeout=120s --for=condition=available deploy/myapp
