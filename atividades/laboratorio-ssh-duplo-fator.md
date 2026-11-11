# Autenticação SSH com Duplo Fator (Chave Assimétrica + Senha)

Este laboratório prático tem como objetivo configurar um ambiente de acesso SSH de alta segurança entre um cliente Windows e um servidor Debian 13. A autenticação exigirá, simultaneamente, uma chave assimétrica **E** a senha do usuário.

> **AVISO:** Esta atividade será avaliada por meio de evidências. É obrigatório capturar **telas inteiras (sem recortes)** em cada etapa sinalizada com o marcador. Imagens cortadas ou sem o prompt com seu nome de usuário personalizado serão descartadas.

---

## Fluxo do Laboratório

1. Validação de Conectividade e Internet
2. Criação do Usuário no Servidor Firewall
3. Geração do Par de Chaves Assimétricas no Windows
4. Criação do Diretório Fixo e Movimentação das Chaves
5. Cópia da Chave Pública para o Servidor Debian
6. Configuração do Serviço SSH no Debian 13
7. Matriz de Testes de Homologação (Execuções Dinâmicas)
8. Instruções de Envio do Relatório Técnico

---

## 1. Validação de Conectividade e Internet

Antes de iniciar qualquer configuração, valide se o ambiente tem conectividade de rede e acesso à internet.

1. No servidor Firewall (Debian), verifique o acesso à internet e a resolução de nomes usando os comandos:

   ```bash
   ping 8.8.8.8
   ping www.google.com
   ```

2. Na máquina cliente (Windows), abra o prompt de comando e faça as mesmas validações:

   ```cmd
   ping 192.168.0.1
   ping 8.8.8.8
   ping www.google.com
   ```

**Observação:** Se ocorrer algum erro, analise-o e corrija-o antes de executar o próximo comando.

## 2. Criação do Usuário no Servidor Firewall

Faça login diretamente no console do servidor Firewall como **root** e crie uma conta local seguindo estritamente o padrão **`nomealunosobrenome`** (letras minúsculas, sem espaços).

- Substitua `nomealunosobrenome` pelo seu próprio nome e sobrenome em todos os comandos. Exemplo: `josesilva`.

Execute o comando a seguir para criar a conta:

```bash
adduser nomealunosobrenome
```

Defina uma senha para o usuário e pressione `Enter` nos campos de informações adicionais para deixá-los em branco.

## 3. Geração do Par de Chaves Assimétricas no Windows

Agora, acesse a máquina Windows. Abra o **MobaXterm** e inicie o terminal embutido clicando em **Start local terminal** (ou usando o atalho de teclado `Ctrl + Alt + T`).

No terminal local do MobaXterm, gere um par de chaves criptográficas com o algoritmo moderno `ED25519` e adicione um comentário que identifique seu usuário:

```bash
ssh-keygen -t ed25519 -C "nomealunosobrenome-notebook"
```

- **Atenção:** Quando o assistente solicitar uma *passphrase*, pressione **Enter** duas vezes seguidas para deixá-la em branco.

📸 **PRINT OBRIGATÓRIO (Evidência 01):** Capture a tela inteira, mostrando o comando de geração e o gráfico geométrico (*randomart*) exibido pelo `ssh-keygen`.

## 4. Criação do Diretório Fixo e Movimentação das Chaves

Para manter as chaves em um local fixo e evitar que permaneçam no ambiente temporário do MobaXterm, crie uma pasta definitiva no drive C: e mova os arquivos para ela.

1. No mesmo terminal do MobaXterm, crie o diretório definitivo no drive C::

   ```bash
   mkdir -p /drives/c/chaves
   ```

2. Mova as chaves do diretório padrão (`~/.ssh`) para o novo local. **Esta etapa é crítica:** mover os arquivos limpa o cache padrão do cliente OpenSSH e permite verificar o comportamento de bloqueio nos testes posteriores.

   ```bash
   mv ~/.ssh/id_ed25519* /drives/c/chaves/
   ```

## 5. Cópia da Chave Pública para o Servidor Debian

Envie a chave pública ao servidor Debian. O utilitário lerá o arquivo indicado e o instalará na conta correta:

```bash
ssh-copy-id -i /drives/c/chaves/id_ed25519.pub nomealunosobrenome@192.168.0.1
```

*Autentique a operação inserindo a senha definida para o usuário no passo 2.*

📸 **PRINT OBRIGATÓRIO (Evidência 02):** Capture a tela inteira do terminal, mostrando a confirmação de sucesso do `ssh-copy-id`.

## 6. Configuração do Serviço SSH no Debian

Retorne ao terminal ou console do servidor Debian como **root**. Abra o arquivo de configuração do serviço SSH:

```bash
nano /etc/ssh/sshd_config
```

Pressione **`Ctrl + W`** para localizar as diretivas. Remova o caractere `#` (descomente-as) e ajuste os parâmetros para que fiquem exatamente como listado abaixo:

```text
PubkeyAuthentication yes
PasswordAuthentication yes
KbdInteractiveAuthentication no
UsePAM no
```

- **`PubkeyAuthentication yes`** -> Permite a autenticação baseada em chave pública.
- **`PasswordAuthentication yes`** -> Mantém a autenticação por senha ativa para que ela funcione como segundo fator.
- **`KbdInteractiveAuthentication no`** -> Desativa a autenticação interativa por teclado.
- **`UsePAM no`** -> Desativa a integração do SSH com os módulos PAM do sistema operacional.

Navegue com a seta para baixo até o **final do arquivo** e insira a regra de combinação obrigatória:

```text
AuthenticationMethods publickey,password
```

- **`AuthenticationMethods publickey,password`** -> Exige a chave pública **E**, em seguida, a senha do usuário. Sem uma chave válida, a senha não será solicitada.

Salve as modificações (`Ctrl + O`, `Enter` e `Ctrl + X`) e reinicie o serviço SSH:

```bash
systemctl restart ssh
```

## 7. Matriz de Testes de Homologação (Execuções Dinâmicas)

Abra uma **nova aba de terminal local** no MobaXterm (mantenha o console do servidor aberto por segurança) e execute os cinco cenários da tabela abaixo para comprovar a eficácia da segurança:

| Cenário | Entrada Aplicada | Comportamento do Servidor | Resultado |
| :--- | :--- | :--- | :--- |
| **A** | Chave correta + senha correta | Aceita a chave e solicita a senha | **Acesso permitido** |
| **B** | Sem chave especificada | Não encontra uma chave válida e bloqueia a tentativa | **Acesso negado** |
| **C** | Chave incorreta/falsa | Rejeita a chave inválida e bloqueia a tentativa | **Acesso negado** |
| **D** | Chave correta + senha errada | Valida a chave, mas rejeita a senha incorreta | **Acesso negado** |
| **E** | Chave do aluno na conta Maria | A chave não está autorizada para a conta de destino | **Acesso negado** |

### Execução Prática e Captura de Evidências

- **📸 Teste A (Acesso legítimo):**

  ```bash
  ssh -i /drives/c/chaves/id_ed25519 nomealunosobrenome@192.168.0.1
  ```

  *Comportamento:* O terminal aceitará a chave e solicitará sua senha. Digite-a corretamente para acessar.

  📸 **PRINT OBRIGATÓRIO (Evidência 03):** Capture a tela mostrando a solicitação da senha após a validação da chave e o login concluído.

- **📸 Teste B (Acesso sem chave):**

  ```bash
  ssh -o IdentitiesOnly=yes -o PreferredAuthentications=publickey nomealunosobrenome@192.168.0.1
  ```

  *Comportamento esperado:* O acesso será bloqueado porque nenhuma chave autorizada será apresentada. O servidor não deverá solicitar a senha.

  📸 **PRINT OBRIGATÓRIO (Evidência 04):** Capture a tela mostrando o erro de autenticação por chave pública.

- **📸 Teste C (Uso de chave incorreta):**

  Gere uma chave temporária diferente e use-a no teste. No terminal local do MobaXterm:

  ```bash
  ssh -i /drives/c/chaves/id_ed25519_falsa -o IdentitiesOnly=yes nomealunosobrenome@192.168.0.1
  ```

  *Comportamento esperado:* O servidor rejeitará a chave temporária, pois ela não foi cadastrada na conta do usuário.

  📸 **PRINT OBRIGATÓRIO (Evidência 05):** Capture a tela mostrando o bloqueio causado pela chave não autorizada.

- **📸 Teste D (Senha incorreta no segundo fator):**

  ```bash
  ssh -i /drives/c/chaves/id_ed25519 nomealunosobrenome@192.168.0.1
  ```

  *Comportamento:* Apresente a chave correta, mas digite uma senha incorreta. O segundo fator será rejeitado.

  📸 **PRINT OBRIGATÓRIO (Evidência 06):** Capture a tela mostrando o erro de autenticação causado pela senha incorreta.

- **📸 Teste E (Isolamento de privilégios entre usuários):**

  ```bash
  ssh -i /drives/c/chaves/id_ed25519 maria@192.168.0.1
  ```

  *Comportamento:* Tente acessar outra conta de usuário (por exemplo, `maria` ou `root`) usando sua chave privada. O servidor negará a autenticação se a chave não estiver autorizada para a conta de destino.

  📸 **PRINT OBRIGATÓRIO (Evidência 07):** Capture a tela mostrando o bloqueio da tentativa de usar a chave em outra conta.

## 8. Instruções de Envio do Relatório Técnico

1. Reúna as **sete capturas de tela** (todas em tela inteira) em ordem cronológica e inclua-as em um relatório técnico explicativo.
2. Salve ou exporte o arquivo exclusivamente no formato **PDF**.
3. Acesse o repositório oficial da disciplina no GitHub: **FATEC-SSO-2026-2**.
4. Acesse o menu **Issues** e crie uma issue seguindo o padrão das outras entregas, com o rótulo `chaves assimetricas`.
5. Anexe o arquivo PDF à descrição da issue, arrastando-o para o campo de descrição.
