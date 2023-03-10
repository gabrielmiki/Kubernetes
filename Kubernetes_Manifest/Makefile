.PHONY: run_website stop_website install_kind create_kind_cluster \
       	connect_registry_to_kind_network connect_registry_to_kind \
	create_kind_cluster_with_registry delete_kind_cluster \
	delete_local_registry

run_website:
	docker build -t explorecalifornia.com . && \
	docker run --rm --name explorecalifornia.com -p 5000:80 -d explorecalifornia.com

stop_website:
	docker stop explorecalifornia.com

install_kind:
	curl --location -o ./kind https://github.com/kubernetes-sigs/kind/releases/download/v0.17.0/kind-linux-amd64 && \
	chmod +x ./kind && \
	./kind --version

create_kind_cluster: install_kind create_docker_registry # install_kubectl
	./kind create cluster --name explorecalifornia.com --config ./kind_config.yml || true && \
	kubectl get nodes

# install_kubectl:
# 	brew install kubectl

create_docker_registry:
	if docker ps | grep -q 'local-registry'; \
	then echo "-----> Local Registry already created"; \
	else docker run --name local-registry -d --restart=always -p 5000:5000 registry:2; \
	fi

connect_registry_to_kind_network:
	docker network connect kind local-registry || true

connect_registry_to_kind: connect_registry_to_kind_network
	kubectl apply -f ./kind_configmap.yml

create_kind_cluster_with_registry:
	$(MAKE) create_kind_cluster && $(MAKE) connect_registry_to_kind

delete_kind_cluster: delete_docker_registry
	./kind delete cluster --name explorecalifornia.com

delete_docker_registry:
	docker stop local-registry && docker rm local-registry
