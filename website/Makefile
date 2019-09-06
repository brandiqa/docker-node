all: setup install start

setup:
	docker volume create nodemodules
install:
	docker-compose -f docker-compose.builder.yml run --rm install
start:
	docker-compose up