CLUSTER_NAME=fullcycle
FOLDER=k8s/
DEPLOYMENT_FILE=deployment.yaml
SERVICE_FILE=service.yaml
SERVICE_NAME=goserver-service

create-cluster:
	kind create cluster --name $(CLUSTER_NAME)
# kind create cluster --name fullcycle

apply-deployment:
	kubectl apply -f $(FOLDER)$(DEPLOYMENT_FILE)
# kubectl apply -f k8s/deployment.yaml

apply-service:
	kubectl apply -f $(FOLDER)$(SERVICE_FILE)
# kubectl apply -f k8s/service.yaml

apply-secret:
	kubectl apply -f $(FOLDER)secret.yaml
	kubectl apply -f $(FOLDER)configmap-env.yaml
	kubectl apply -f $(FOLDER)configmap-family.yaml
# kubectl apply -f k8s/secret.yaml
# kubectl apply -f k8s/configmap-env.yaml
# kubectl apply -f k8s/configmap-family.yaml

.PHONY: create-cluster apply-deployment

port-forward:
	kubectl port-forward $(SERVICE_NAME) 4030:4000
# kubectl port-forward goserver-service 4030:4000

destroy-all:
	kubectl delete -f $(FOLDER)$(DEPLOYMENT_FILE)
	kubectl delete -f $(FOLDER)$(SERVICE_FILE)
	kind delete cluster --name $(CLUSTER_NAME)
# kind delete cluster --name fullcycle
# kubectl delete -f k8s/deployment.yaml
# kubectl delete -f k8s/service.yaml

docker-build:
	docker build . -t andrejr971/hello-go:v$(version)
# Para executar esse comando passando a versão pelo terminal:
# make docker-build version=6

docker-push:
	docker push andrejr971/hello-go:v$(version)
# Para executar esse comando passando a versão pelo terminal:
# make docker-push version=6

dev-start: create-cluster apply-secret apply-deployment apply-service


