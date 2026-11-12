# 🎼 Laboratório: Orquestração com Docker Compose

Nesta aula, vamos aprender a gerenciar e conectar múltiplos serviços de forma organizada utilizando um único arquivo de configuração com o **Docker Compose**.

---

## 🚀 1. Subindo um Projeto Web com Docker Compose

Em vez de digitar comandos `docker run` longos com várias flags (`-d`, `-p`, `-v`, `--name`), vamos definir toda a configuração do nosso servidor web dentro de um arquivo YAML.

### 1.1 Preparando a pasta do projeto
No **Git Bash**, crie uma pasta para este laboratório (ou utilize a pasta do projeto web clonado do GitHub na aula anterior):

```bash
mkdir C:/lab-compose
cd C:/lab-compose
echo "<h1>Projeto Web com Docker Compose!</h1>" > index.html
```

### 1.2 Criando o arquivo `docker-compose.yml`
Dentro da pasta `C:/lab-compose`, crie um arquivo chamado exatamente **`docker-compose.yml`** (use o VS Code ou o comando abaixo) com a seguinte estrutura:

```yaml
services:
  servidor-web:
    image: httpd:alpine
    container_name: apache-compose
    ports:
      - "8080:80"
    volumes:
      - ./:/usr/local/apache2/htdocs/
```

> **Entendendo o arquivo:**
> * `services`: Define a lista de contêineres que farão parte da nossa aplicação.
> * `servidor-web`: É o nome do serviço dentro do Compose.
> * `image`: A imagem que será baixada e utilizada (`httpd:alpine`).
> * `container_name`: Define o nome fixo do contêiner.
> * `ports`: Equivalente à flag `-p 8080:80`.
> * `volumes`: Equivalente à flag `-v` (o `./` aponta para a pasta atual `C:/lab-compose`).

### 1.3 Executando e gerenciando o Compose
No **Git Bash**, certifique-se de estar dentro da pasta `C:/lab-compose` e execute os comandos abaixo:

```bash
# Verifica a versão do Docker Compose
docker compose version

# Sobe os serviços configurados em segundo plano
docker compose up -d

# Lista os contêineres que estão rodando neste Compose
docker compose ps

# Visualiza os logs do servidor web
docker compose logs
```

Acesse **`http://localhost:8080`** no navegador para confirmar que o projeto web está no ar.

### 1.4 Derrubando o ambiente
Para parar e remover completamente os contêineres e redes criados pelo Compose:

```bash
docker compose down
```

---

## 🗄️ 2. Exercícios Práticos — Subindo e Testando Banco de Dados

**2.** Agora você vai praticar a criação de um ambiente com **Docker Compose** para subir um banco de dados **MySQL** e validar seu funcionamento criando tabelas e consultando dados. Para isso, você vai pesquisar como criar um arquivo `docker-compose.yml` para o MySQL, subir o contêiner e acessar o banco de dados para realizar operações básicas. Lembrando que para realizar essa tarefa não é necessário criar outros arquivos além do `docker-compose.yml`, mas você pode criar scripts SQL para facilitar a criação das tabelas e inserção de dados, caso queira.

**3.** Agora faça o mesmo para o **MariaDB**. Crie um arquivo `docker-compose.yml` para subir o MariaDB, acesse o banco de dados e realize operações básicas como criar tabelas, inserir dados e consultar informações.

> Todas as etapas devem ser documentadas com prints de tela mostrando o passo a passo, incluindo o conteúdo do arquivo `docker-compose.yml`, os comandos SQL utilizados e os resultados das consultas realizadas. Lembre-se de personalizar os nomes dos contêineres, pastas e dados com o seu nome!
>
> **Entrega:** Reúna tudo em um único arquivo **PDF** (incluindo prints de tela e explicações do passo a passo) e envie na **Issue** da atividade neste repositório do GitHub com a label `atividade-docker-compose`.
