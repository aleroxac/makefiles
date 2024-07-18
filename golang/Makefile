## ---------- UTILS
default: help

.PHONY: help
help: ## Show this menu
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

.PHONY: clean
clean: ## Clean all temp files
	@sudo rm -f coverage.* .run .build


## ---------- FUNCTIONS
## setup workspace
define setup
	for folder in .run .build; do \
		[ -d $1 ] && mkdir $1 || true; \
		cp main.go .build; \
	done
endef

## teardown workspace
define teardown
	for folder in .run .build; do \
		[ -d $1 ] && rm -rf $1 || true; \
	done
endef



## ---------- SETUP
.PHONY: install
install: ## install requirements
	@go install github.com/cosmtrek/air@latest
	@go install github.com/swaggo/swag/cmd/swag@latest
	@go mod tidy



## ---------- FORMAT & LINT
.PHONY: fmt
fmt: ## format the code
	@go fmt ./...

.PHONY: vet
vet: ## run static analysis
	@go vet ./...



## ---------- BUILD
.PHONY: build
build: ## Build the code
	@$(call setup, .build)
	@go build -o main .
	@$(call teardown, .build)

.PHONY: buildc
buildc: ## Build the container image
	@docker build --build-arg ENV=dev -t aleroxac/myapp:v1 -f Dockerfile .



## ---------- TESTS
.PHONY: test
test: ## run unit-tests
	@go test -v -run ^Test ./... -coverprofile coverage.out
	@go tool cover -html=coverage.out -o coverage.html

.PHONY: bench
bench: ## run benchmarks
	@go test -v -benchmem -run ^Benchmark ./...



## ---------- MAIN
.PHONY: docs
docs:
	@swag init

.PHONY: run-with-docs
run-with-docs: docs ## build docs and run the app locally
	@go run main.go

.PHONY: air
air: ## run the app locally, with live-reaload by air
	@air

.PHONY: air-with-docs
air-with-docs: docs ## build docs and run the app locally, with live-reaload by air
	@air

.PHONY: run
run: ## run the app
	@go run main.go

.PHONY: runc
runc: ## run the app via docker
	@docker run -d --name=myapp -p 8080:8080 --env-file=${PWD}/.env aleroxac/myapp:v1
