.DEFAULT_GOAL:=help
ISTIO_HOME:=../../istio-1.0.4

prepare: ## Prepare Istio installation for demo
	@echo "Installing Istio"
	kubectl apply -f $(ISTIO_HOME)/install/kubernetes/helm/istio/templates/crds.yaml
	kubectl apply -f $(ISTIO_HOME)/install/kubernetes/istio-demo.yaml
	@echo "Installing Bookinfo application"
	kubectl label namespace default istio-injection=enabled
	kubectl apply -f $(ISTIO_HOME)/samples/bookinfo/platform/kube/bookinfo.yaml
	kubectl apply -f $(ISTIO_HOME)/samples/bookinfo/networking/bookinfo-gateway.yaml
	kubectl apply -f $(ISTIO_HOME)/samples/bookinfo/networking/destination-rule-all.yaml
	kubectl apply -f demo-manifests/defaults.yaml

open: ## Open Bookinfo application in browser
	@open http://localhost/productpage

open-jaeger: ## Open Jaeger in browser
	@open http://localhost:16686
	kubectl port-forward -n istio-system $$(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686

canary: ## Test version 2 of reviews service (black stars) with user "joe"
	kubectl apply -f demo-manifests/canary-reviews-v2.yaml

canary-error: ## Version 1 of ratings service with user "joe.p" doesn't work
	kubectl apply -f demo-manifests/canary-ratings-v1-errors.yaml

clean: ## Delete all Istio resources
	$(ISTIO_HOME)/samples/bookinfo/platform/kube/cleanup.sh
	kubectl label namespace default istio-injection-
	kubectl delete -f $(ISTIO_HOME)/install/kubernetes/istio-demo.yaml
	kubectl delete -f $(ISTIO_HOME)/install/kubernetes/helm/istio/templates/crds.yaml -n istio-system

help: ## Display this help. Thanks to https://suva.sh/posts/well-documented-makefiles/
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make \033[36m<target>\033[0m\n\nTargets:\n"} /^[a-zA-Z_-]+:.*?##/ { printf "  \033[36m%-12s\033[0m %s\n", $$1, $$2 }' $(MAKEFILE_LIST)
