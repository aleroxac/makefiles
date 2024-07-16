conf ?= .env
include $(conf)
export $(shell sed 's/=.*//' $(conf))



## ---------- UTILS
.PHONY: help
help: ## Show this menu
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)



## ---------- HELPERS
.PHONY: create_dotenv
create_dotenv: ## Create dotenv
	scripts/compose_dotenv.sh .env.example > .env

.PHONY: import_dotenv
import_dotenv: ## Import dotenv
	$(eval include .env)
	$(eval export)



## ---------- MAIN
.PHONY: run
run: create_dotenv import_dotenv ## run the script
	@bash script.sh
