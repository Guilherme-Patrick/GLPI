# Instalação e configuração do GLPI Agent no Linux

Este guia descreve o processo para instalar e configurar o GLPI Agent em uma máquina Linux, permitindo o envio das informações de inventário para o servidor GLPI.

>  **Observação:**
> O servidor GLPI já deve estar instalado e funcionando. Neste procedimento será configurado apenas o agente na máquina Linux.

---

## Pré-requisitos

- Máquina Linux
- Acesso administrativo (`root` ou `sudo`)
- Acesso à rede
- Endereço do servidor GLPI

---

##  Passo a passo

###  1. Verificar a conexão com o servidor GLPI

Antes de instalar o agente, verificar se a máquina Linux consegue acessar o servidor GLPI.

Executar:

```bash
ping glpi.in.iti.br
```
Ou:

```bash
ping 192.168.16.52
```

---

###  2. Baixar o GLPI Agent

Baixar o instalador do agente:

```bash
wget -O glpi-agent-installer.pl https://github.com/glpi-project/glpi-agent/releases/download/1.19/glpi-agent-1.19-linux-installer.pl && sudo perl glpi-agent-installer.pl --server="https://glpi.in.iti.br/" --tag="CGISE" --no-ssl-check --runnow
```

---

###  3. Instalar o GLPI Agent

Executar o instalador:

```bash
sudo perl glpi-agent-linux-installer.pl
```

Após a instalação, verificar a versão do agente:

```bash
glpi-agent --version
```

---

###  4. Configurar o servidor GLPI

Acessar o diretório de configuração do agente:

```bash
cd /etc/glpi-agent
```

Editar o arquivo de configuração:

```bash
sudo vim /etc/glpi-agent/agent.cfg
```

Adicionar o endereço do servidor GLPI:

```text
server = https://glpi.in.iti.br/
tag = CGISE
no-ssl-check = 1
```


Após realizar a alteração, salvar o arquivo.

---

###  5. Verificar o serviço do GLPI Agent

Verificar se o serviço está funcionando:

```bash
systemctl status glpi-agent
```

Caso o serviço esteja parado, iniciar:

```bash
sudo systemctl start glpi-agent
```

Configurar para iniciar automaticamente com o sistema:

```bash
sudo systemctl enable glpi-agent
```

---

###  6. Executar o inventário

Para testar a configuração e enviar o inventário para o servidor GLPI, executar:

```bash
sudo glpi-agent --debug --logger=stderr --force
```

Após a execução, verificar se não foram apresentados erros.

---

## Validação

Após finalizar, verificar:

- [ ] GLPI Agent instalado
- [ ] Servidor GLPI configurado no `agent.cfg`
- [ ] Serviço do GLPI Agent funcionando
- [ ] Inventário executado
