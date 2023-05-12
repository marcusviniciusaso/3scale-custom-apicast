# Criando API Gateway (APICast) auto-gerenciado

Neste documento vamos passar pelo mínimos necessário para um API Gateway (APICast) auto-gerenciado no 3scale. Quando você uma instância do API Manager, 
ela já vem com um API Gateway padrão. Porém se você quiser ter controle total sobre o API Gateway, é necessário criar um API Gateway auto-gerenciado.

## Pré-requisitos
- Red Hat OpenShift Container Platform 4.x;
- Operator Red Hat Integration - 3scale instalado;
- Operator Red Hat Integration - 3scale APICast gateway instalado.  

![installed-operators.png](/resources/img/installed-operators.png "Installed Operators")

## Criação de conta de serviço

A conta de serviço vai ser utilizada para baixar as imagens. É possível criar a conta de serviço [neste link](https://access.redhat.com/terms-based-registry/#/).

Clique em "New Service Account":
![new-service-account.png](/resources/img/new-service-account.png "New Service Account")

Preencha o nome e clique em "Create":
![service-account-form.png](/resources/img/service-account-form.png "Service Account Form")

Service account criado:
![service-account-created.png](/resources/img/service-account-created.png "Service Account Created")

Faça o download do yaml da secret na aba "OpenShift Secret" das informações do token:
![download-secret.png](/resources/img/download-secret.png "Download Secret")

## Criação de secret

Utilize o yaml baixado no passo anterior para criar a secret.

Criar a secret:
```
oc create -f 3scale-pull-secret.yaml -n marolive-3scale
```
Coloque o namespace correto.

Vincular a secret ao service account default:
```
oc secrets link default 3scale-pull-secret --for=pull
```

Vincular a secret ao service account builder:
```
oc secrets link builder 3scale-pull-secret
```

## Criação de API Manager

Entre no operator "Operator Red Hat Integration - 3scale" para criar o APIManager:
![create-apimanager.png](/resources/img/create-apimanager.png "Create APIManager")

Na tela de criação coloque no [yaml do API Manager](apimanager.yaml) os valores corretos:
![apimanager-yaml-form.png](/resources/img/apimanager-yaml-form.png "APIManager yaml")
Atenção aos valores dos campos "namespace" e "wildcardDomain". Coloque o domínio correto do OpenShift.