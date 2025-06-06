## Referência de Comandos Kubernetes

Este documento fornece uma referência concisa para gerenciar clusters e recursos Kubernetes usando os comandos `kind` e `kubectl`.

---

### Explicação dos Tipos de Objetos

No Kubernetes, os principais tipos de objetos utilizados para gerenciar aplicações e infraestrutura incluem:

- **Pod:** A menor unidade implantável no Kubernetes, representa um ou mais containers que compartilham armazenamento, rede e especificações sobre como rodar os containers.
- **ReplicaSet:** Garante que um número especificado de réplicas de um pod estejam rodando a qualquer momento. É usado para manter a alta disponibilidade das aplicações.
- **Service:** Um recurso que expõe um conjunto de pods como um serviço de rede estável, permitindo comunicação interna ou externa e balanceamento de carga.
- **Deployment:** Gerencia ReplicaSets e permite atualizações declarativas para pods e ReplicaSets.

---

### Gerenciamento de Cluster

- **Listar todos os clusters:**
  ```sh
  kind get clusters
  ```
- **Mostrar detalhes do contexto do cluster:**
  ```sh
  kubectl cluster-info --context kind-<nome>
  ```
- **Criar um cluster usando um arquivo de configuração:**
  ```sh
  kind create cluster --config=k8s/kind.yaml --name=<nome>
  ```

---

### Gerenciamento de Recursos

#### Listar Recursos

- **Listar todos os pods:**
  ```sh
  kubectl get pods
  ```
- **Listar todos os ReplicaSets:**
  ```sh
  kubectl get replicasets
  ```
- **Descrever um pod específico:**
  ```sh
  kubectl describe pod <nome>
  ```

#### Criar Recursos

- **Criar um pod a partir de um arquivo YAML:**
  ```sh
  kubectl apply -f k8s/pod.yaml
  ```
- **Criar um ReplicaSet a partir de um arquivo YAML:**
  ```sh
  kubectl apply -f k8s/replicaset.yaml
  ```

#### Deletar Recursos

- **Deletar um pod pelo nome:**
  ```sh
  kubectl delete pod <nome>
  ```
- **Deletar um ReplicaSet pelo nome:**
  ```sh
  kubectl delete replicasets <nome>
  ```

---

### Gerenciamento de Rollout & Revisão

- **Ver histórico de rollout:**
  ```sh
  kubectl rollout history <tipo de objeto> <nome>
  ```
- **Reverter para a versão anterior:**
  ```sh
  kubectl rollout undo <tipo de objeto> <nome>
  ```
- **Reverter para uma revisão específica:**
  ```sh
  kubectl rollout undo <tipo de objeto> <nome> --to-revision=<número da revisão>
  ```

---

## Documentação do Serviço Kubernetes

O Service do Kubernetes é definido em `k8s/service.yaml` e expõe a aplicação `goserver` dentro do cluster. Ele fornece um endpoint de rede estável, permitindo comunicação interna ou externa com os pods `goserver`, além de realizar balanceamento de carga e descoberta de serviços.

### Tipos de Service no Kubernetes

O Kubernetes oferece diferentes tipos de Service para expor aplicações:

- **ClusterIP**: (padrão) Expõe o serviço em um IP interno do cluster. Só é acessível de dentro do cluster.
- **NodePort**: Expõe o serviço em uma porta estática em cada nó do cluster, permitindo acesso externo via `<IP-do-nó>:<NodePort>`.
- **LoadBalancer**: Cria um balanceador de carga externo (em provedores de nuvem compatíveis) e direciona o tráfego para o serviço.
- **ExternalName**: Mapeia o serviço para um nome DNS externo, sem criar proxy ou IPs no cluster.


### Uso

- **Aplicar a configuração do serviço:**
  ```sh
  kubectl apply -f k8s/service.yaml
  ```
- **Verificar a criação e o status do serviço:**
  ```sh
  kubectl get services
  ```

### Acessando o Serviço Localmente

- **Redirecionar a porta local para o serviço `goserver-service`:**
  ```sh
  kubectl port-forward svc/goserver-service 8000:80
  ```
  Isso permite acessar o serviço `goserver-service` na porta local `8000`, encaminhando o tráfego para a porta `80` do serviço dentro do cluster.

# Documentação do Comando `kubectl proxy --port=9000`

Este comando inicia um proxy local que permite acessar a API do Kubernetes de forma segura a partir do seu computador. O proxy escuta na porta 9000 e encaminha as solicitações para o servidor da API do cluster Kubernetes ao qual você está conectado.

---

## Kubernetes Dashboard (Interface Gráfica)

O Kubernetes possui uma interface web oficial chamada **Kubernetes Dashboard**. Com ela, é possível visualizar e gerenciar recursos do cluster de forma gráfica.

### Como instalar e acessar o Dashboard

1. **Instale o Dashboard:**
   ```sh
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
   ```

2. **Crie um usuário administrador:**
   ```sh
   cat <<EOF | kubectl apply -f -
   apiVersion: v1
   kind: ServiceAccount
   metadata:
     name: admin-user
     namespace: kubernetes-dashboard
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: admin-user
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: cluster-admin
   subjects:
   - kind: ServiceAccount
     name: admin-user
     namespace: kubernetes-dashboard
   EOF
   ```

3. **Gere o token de acesso:**
   ```sh
   kubectl -n kubernetes-dashboard create token admin-user
   ```

4. **Inicie o proxy:**
   ```sh
   kubectl proxy
   ```

5. **Acesse no navegador:**
   ```
   http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
   ```
   Faça login usando o token gerado.

