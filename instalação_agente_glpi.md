# Instalação e configuração do GLPI Agent no Linux

Este guia descreve o processo para instalar e configurar o GLPI Agent em uma máquina Linux, permitindo o envio das informações de inventário para o servidor GLPI.

>  **Observação:**
> Caso o DNS não esteja configurado, deve ser adicionado o ip e o dominio no hosts

Execute:
 
```bash
sudo vim /etc/hosts
```
Adicione: 
```bash
192.168.16.52    glpi.in.iti.br
```

##  Passo a passo


###  1. Baixar o GLPI Agent

Baixar o instalador do agente:

```bash
sudo wget -O glpi-agent-installer.pl https://github.com/glpi-project/glpi-agent/releases/download/1.19/glpi-agent-1.19-linux-installer.pl && sudo perl glpi-agent-installer.pl --server="https://glpi.in.iti.br/" --tag="CGISE" --no-ssl-check --runnow
```
---

###  2. Executar o inventário

Para testar a configuração e enviar o inventário para o servidor GLPI, executar:

```bash
sudo glpi-agent --debug --logger=stderr --force
```
