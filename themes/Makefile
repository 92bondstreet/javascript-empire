SHELL := /bin/bash

YEAR = $$(date +'%Y')
ROOT_DIR:=$(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))

browse: ## browse stockx.com for the current year and past year with given parameters
	node $(ROOT_DIR)/bin/index.js
	sleep 30
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$(YEAR)&currency=EUR&sort=price_premium"
	sleep 30
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$(YEAR)&currency=EUR&sort=average_deadstock_price"
	sleep 30
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$(YEAR)&currency=EUR&sort=most-active"
	sleep 30
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$$(( $(YEAR) - 1 ))&currency=EUR&sort=price_premium"
	sleep 30
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$$(( $(YEAR) - 1 ))&currency=EUR&sort=average_deadstock_price"
	sleep 30
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$$(( $(YEAR) - 1 ))&currency=EUR&sort=most-active"

browse-current: ## browse stockx.com for the current year with given parameters
	node $(ROOT_DIR)/bin/index.js
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$(YEAR)&currency=EUR&sort=price_premium"
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$(YEAR)&currency=EUR&sort=average_deadstock_price"
	node $(ROOT_DIR)/bin/index.js --no-clean -p "_tags=sneakers&productCategory=sneakers&order=DESC&year=$(YEAR)&currency=EUR&sort=most-active"

build: ## build and up containers
	docker-compose up -d --build

down: ## down all containers
	docker-compose down

export: ## export kibana objects
	sh .kibana/export.sh $(filter-out $@,$(MAKECMDGOALS))

import: ## import kibana objects
	sh .kibana/import.sh $(filter-out $@,$(MAKECMDGOALS))

install: ## install missing tools
	unameOut="$(uname -s)"
	case "${unameOut}" in
	Linux*)     apt-get install jq;;
	Darwin*)    brew install jq;;
	esac

run: ## run a cron to index stockx activities
	docker rm elastockx -f || true
	make build

help: ## This help dialog.
	@IFS=$$'\n' ; \
	intro_lines=(`fgrep -h "###" $(MAKEFILE_LIST) | fgrep -v fgrep | sed -e 's/###//'`); \
	for line in $${intro_lines[@]}; do \
		printf "%s\n" $$line; \
	done; \
	help_lines=(`fgrep -h -e "##" $(MAKEFILE_LIST) | fgrep -v "###" | fgrep -v fgrep | sed -e 's/\\$$//' | sed -e 's/##/:/'`); \
	for help_line in $${help_lines[@]}; do \
		IFS=$$':' ; \
		help_split=($$help_line) ; \
		help_command=`echo $${help_split[0]} | sed -e 's/^ *//' -e 's/ *$$//'` ; \
		help_info=`echo $${help_split[2]} | sed -e 's/^ *//' -e 's/ *$$//'` ; \
		printf '\033[36m'; \
		printf "%-30s %s" $$help_command ; \
		printf '\033[0m'; \
		printf "%s\n" $$help_info; \
	done

.DEFAULT_GOAL := help
