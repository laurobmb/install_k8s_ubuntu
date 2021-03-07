## DICA
source <(kubectl completion bash)

## Ajuste no metritcs

Edite o deployment do metrics que fina no namespace kube-system

> kubectl -n kube-system edit deployments metrics-server

	spec:
      containers:
        command:
          - /metrics-server
          - --metric-resolution=5s
          - --kubelet-preferred-address-types=InternalIP
          - --kubelet-insecure-tls

> kubectl top nodes
> kubectl top pods

## Ajuste no nginx 

Coloque os endereços IPv4 que devem responder pelo acesso aos serviços internos do cluster

> kubectl -n ingress-nginx get svc

> kubectl -n ingress-nginx edit svc my-ingress-nginx-ingress

	spec:
	  externalIPs:
	    - 192.168.123.206
	    - 192.168.123.218
	    - 192.168.123.144

> kubectl -n ingress-nginx get svc




