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