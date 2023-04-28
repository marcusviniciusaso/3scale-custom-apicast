# Criando APICast Customizado

Neste documento vamos passar pelo mínimos necessário para um APICast customizado no 3scale.

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

Faça o download do yaml da secret na aba "OpenShift Secret" das informações do token:
![download-secret.png](/resources/img/download-secret.png "Download Secret")

## Criação de secret

Utilize o yaml baixado no passo anterior para criar a secret.

Criar a secret:
```
oc create -f 3scale-pull-secret.yaml -n marolive-3scale
```

Vincular a secret ao service account default:
```
oc secrets link default 3scale-pull-secret --for=pull
```

Vincular a secret ao service account builder:
```
oc secrets link builder 3scale-pull-secret
```