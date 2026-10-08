# Laboratório: Acesso SSH com Cliente Windows e MobaXterm

Roteiro prático instalação do servidor SSH no Linux (Firewall)

---

## 1. Configuração e Inicialização do Servidor Linux (FW)

1. **Configurar as Placas de Rede** no VirtualBox:
   - **Interface 1 (Primeira):** Modo NAT (para acesso à Internet).
   - **Interface 2 (Segunda):** Rede Interna (para comunicação com o cliente Windows).
2. **Ligar a máquina** Linux (FW).

## 2. Testes de Conectividade no Servidor Linux

Valide se o servidor possui acesso à internet e resolução de nomes (DNS):

```bash
# Testar conectividade direta por IP
ping -c 4 8.8.8.8

# Testar a resolução de nomes (DNS)
ping -c 4 www.google.com.br
```

## 3. Instalação do Serviço SSH e Criação de Usuários

Execute os comandos abaixo no terminal do Linux para preparar o ambiente de acesso:

```bash
# Atualizar a lista de pacotes e instalar o servidor SSH
sudo apt update && sudo apt install ssh -y

# Verificar o serviço do SSH
sudo systemctl status sshd.service

# Criar os usuários para os testes de acesso
sudo adduser aluno
sudo adduser maria
sudo adduser jose
```

## 4. Configuração do Cliente Windows

1. Garanta que a placa de rede da máquina virtual Windows esteja configurada no modo **Rede Interna** e ligue a VM.
2. No Windows, abra o menu Executar (`Win + R`), digite `ncpa.cpl` e pressione `Enter` para abrir as Conexões de Rede.
3. Altere as propriedades do protocolo **IPv4** da placa de rede interna manualmente com os seguintes dados:
   - **Endereço IP:** `192.168.0.100`
   - **Máscara de Sub-rede:** `255.255.255.0`
   - **Gateway Padrão:** `192.168.0.1`
   - **Servidor DNS:** (Insira o IP do DNS da sua máquina real/hospedeira)
4. Abra o Prompt de Comando (`cmd`) no Windows e teste a conectividade com o IP interno do servidor Linux:
   ```cmd
   ping 192.168.0.1
   ```

## 5. Instalação e Acesso via MobaXterm

1. No cliente Windows, acesse o site oficial e baixe a versão Portable: [Download MobaXterm Home Edition](https://mobaxterm.mobatek.net/download-home-edition.html).
2. Descompacte o arquivo `.zip` baixado, abra a pasta e execute o **MobaXterm**.
3. No terminal local do MobaXterm (ou iniciando uma nova sessão SSH), conecte-se ao servidor utilizando o usuário criado:
   ```bash
   ssh aluno@192.168.0.1
   ```
4. Insira a senha definida para o usuário `aluno`.

## 6. Auditoria de Conexão no Servidor

Após realizar o acesso pelo Windows, retorne ao terminal do servidor Linux ou verifique na própria sessão ativa quem está conectado executando:

```bash
who
```
*O comando exibirá a lista de usuários logados no sistema e o IP de origem da conexão.*
