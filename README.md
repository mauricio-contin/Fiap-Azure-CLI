# Fiap-Azure-CLI

Laboratório: Criando uma VM na Azure com Apache utilizando Azure CLI 

  

Objetivo 

  

Neste laboratório, você aprenderá a usar a Azure CLI para criar uma Máquina Virtual (VM) na Azure, instalar o servidor Apache e liberar a porta necessária para conexão com a internet. 

  

Pré-requisitos 

  

1. Conta na Microsoft Azure. 

2. Azure CLI instalada. Se não estiver instalada, siga as instruções aqui. 

 

Primeiros passos: 

  

1. Login na Azure 

Primeiro, faça login na sua conta Azure usando o comando: 

az login 
  

2. Criação de um Grupo de Recursos 

Crie um grupo de recursos para organizar os recursos que você criará. Substitua `<ResourceGroupName>` e `<Location>` pelos valores desejados: 

az group create --name <ResourceGroupName> --location <Location> 


3. Criação da Máquina Virtual   

Crie uma nova VM. Substitua `<ResourceGroupName>`, `<VMName>`, `<Username>`, e `<Password>` pelos valores desejados: 

az vm create --resource-group <ResourceGroupName> --name <VMName> --image UbuntuLTS --admin-username <Username> --admin-password <Password> --size Standard_B1s --authentication-type password 

4. Abrir a Porta 80 (HTTP) 

Para permitir tráfego HTTP (porta 80) para a VM, execute o seguinte comando: 

az vm open-port --port 80 --resource-group <ResourceGroupName> --name <VMName> 


5. Conectar-se à VM 

Obtenha o endereço IP público da VM: 

az vm show --resource-group <ResourceGroupName> --name <VMName> --show-details --query [publicIps] --output tsv 

Use o endereço IP obtido para se conectar à VM via SSH: 

ssh Username@PublicIP

6. Instalação do Apache

Após conectar-se à VM, execute os seguintes comandos para instalar o Apache:   

sudo apt update 
sudo apt install -y apache2 
sudo systemctl start apache2 
sudo systemctl enable apache2 
  
7. Verificar a Instalação 

No navegador da web, digite o endereço IP público da VM. Você deve ver a página padrão do Apache confirmando que ele está instalado e funcionando. 


Antes de finalizar o Laboratório, iremos executar o comando para remover tudo que foi criado, evitando o consumo de recursos e disperdicios: 
 
az group delete --name <ResourceGroupName> --yes --no-wait 
