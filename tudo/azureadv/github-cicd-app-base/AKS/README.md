# Provisionamento de AKS com Terraform

Este projeto usa Terraform para provisionar um cluster Kubernetes no Azure Kubernetes Service (AKS) e configurar o acesso ao cluster para verificar o estado dos nós.

## 📋 Pré-requisitos

1. **Azure CLI** - Certifique-se de ter o Azure CLI instalado e configurado para se autenticar com sua conta do Azure.
   - [Instalar Azure CLI](https://docs.microsoft.com/pt-br/cli/azure/install-azure-cli)
   - Para autenticar: `az login`

2. **Terraform** - Certifique-se de ter o Terraform instalado.
   - [Instalar Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

3. **kubectl** - Instale o `kubectl` para interagir com o cluster Kubernetes.
   - [Instalar kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## 🚀 Configuração Inicial

1. Clone o repositório e navegue até o diretório do projeto:

```bash
git clone https://github.com/ecastrojr/github-cicd-app-base
cd github-cicd-app-base
```

2. Configurar Variáveis

Certifique-se de que as variáveis em variables.tf estão configuradas conforme necessário para o seu ambiente. Você pode modificá-las diretamente ou usar um arquivo terraform.tfvars para personalizar as variáveis.

Exemplo de terraform.tfvars:
```bash
resource_group_location = "eastus"
resource_group_name     = "rg-aks-dio"
cluster_name            = "cluster-aks-dio"
node_count              = 1
username                = "azureadmin"
```

## ⚙️ Passo a Passo para Provisionar o AKS
1. Inicializar o Terraform

Inicialize o Terraform para baixar os provedores necessários:
```bash
terraform init
```

2. Executar o Plano de Terraform

Para visualizar o plano de criação dos recursos no Azure, execute:
```bash
terraform plan
```
3. Aplicar o Plano

Aplique o plano para provisionar o AKS e outros recursos especificados:
```bash
terraform apply
```
 📌 Digite **yes** quando solicitado para confirmar a criação dos recursos.

4. Configurar o kubectl

Após o provisionamento, configure o kubectl para se conectar ao cluster AKS:
```bash
echo "$(terraform output kube_config)" > ./azurek8s
export KUBECONFIG=./azurek8s
```
ou
```bash
az aks get-credentials --resource-group $(terraform output -raw resource_group_name) --name $(terraform output -raw kubernetes_cluster_name)
```
5. Verificar os Nós do Cluster

Verifique os nós do cluster com o comando abaixo:
```bash
kubectl get nodes
```
Esse comando deve retornar os nós do cluster, confirmando que o AKS foi provisionado com sucesso.

## 🧹 Destruir o Ambiente

 Para remover todos os recursos criados pelo Terraform, execute:
```bash
terraform destroy
```
Digite **yes** para confirmar a destruição dos recursos.
Observações

### 📌 Observações

-    Certifique-se de ter privilégios administrativos no Azure para criar e destruir recursos.
-    Importante: O comando terraform destroy remove todos os recursos provisionados por este código, então use-o com cuidado.

## 🧑‍💻 Autor

Este repositório foi criado e mantido por [Alex Castro](https://github.com/ecastrojr).