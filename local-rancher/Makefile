SHELL := /bin/bash

RANCHER_IMAGE := rancher/rancher
RANCHER_CONTAINER := my-local-rancher


kill:
	@set -euo pipefail; \
	CONTAINER_ID=$$(docker ps -q -a -f "name=$(RANCHER_CONTAINER)"); \
	if [[ -n $$CONTAINER_ID ]]; then \
		docker stop $$CONTAINER_ID; \
		docker rm --volumes $$CONTAINER_ID; \
	fi;

clean: kill
	@set -euo pipefail; \
	IMAGE_ID=$$(docker images -q "$(RANCHER_IMAGE)"); \
	if [[ -n $$IMAGE_ID ]]; then \
		docker rmi -f $$IMAGE_ID; \
	fi;

run:
	docker run --privileged -d --restart=unless-stopped -p 8080:80 -p 8443:443 \
		--volume $$(pwd)/helm:/var/lib/rancher/helm \
		--name $(RANCHER_CONTAINER) $(RANCHER_IMAGE)

bootstrap-password:
	@docker logs $(RANCHER_CONTAINER) 2>&1 | grep "Bootstrap Password:"

exec:
	@docker exec -it $(RANCHER_CONTAINER) bash
