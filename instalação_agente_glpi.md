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
ping IP_DO_SERVIDOR_GLPI
```

Também pode ser realizado um teste de acesso ao servidor:

```bash
curl -I http://IP_DO_SERVIDOR_GLPI/
```

---

###  2. Baixar o GLPI Agent

Baixar o instalador do agente:

```bash
wget -O glpi-agent-linux-installer.pl https://github.com/glpi-project/glpi-agent/releases/latest/download/glpi-agent-linux-installer.pl
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
server = http://IP_DO_SERVIDOR_GLPI/front/inventory.php
```

Exemplo:

```text
server =  https://glpi.in.iti.br
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

###  7. Verificar o computador no GLPI

Após executar o inventário:

- Acessar o servidor GLPI.
- Ir em **Ativos**.
- Acessar **Computadores**.
- Procurar pelo nome da máquina Linux.

A máquina deverá aparecer com as informações coletadas pelo agente.

---

##  Caso o computador não apareça

Verificar o status do serviço:

```bash
systemctl status glpi-agent
```

Verificar os logs:

```bash
journalctl -u glpi-agent
```

Executar novamente o inventário em modo de depuração:

```bash
sudo glpi-agent --debug --logger=stderr --force
```

Verificar também se a máquina consegue acessar o servidor GLPI:

```bash
curl -I http://IP_DO_SERVIDOR_GLPI/
```

---

## Validação

Após finalizar, verificar:

- [ ] GLPI Agent instalado
- [ ] Servidor GLPI configurado no `agent.cfg`
- [ ] Serviço do GLPI Agent funcionando
- [ ] Inventário executado
- [ ] Máquina Linux aparecendo no GLPI
