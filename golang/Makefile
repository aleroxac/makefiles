## ---------- UTILS
.PHONY: help
help: ## Show this menu
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

.PHONY: clean
clean: ## Clean all temp files
	@sudo rm -f coverage.* 



## ---------- SETUP
.PHONY: install
install: ## install requirements
	@go mod tidy



## ---------- FORMAT & LINT
.PHONY: fmt
fmt: ## format the code
	@go fmt ./...

.PHONY: vet
vet: ## run static analysis
	@go vet ./...



## ---------- TESTS
.PHONY: test
test: ## run unit-tests
	@go test -v -run ^Test ./... -coverprofile coverage.out
	@go tool cover -html=coverage.out -o coverage.html

.PHONY: bench
bench: ## run benchmarks
	@go test -v -benchmem -run ^Benchmark ./...



## ---------- MAIN
.PHONY: run
run: ## run the app
	@go run main.go
