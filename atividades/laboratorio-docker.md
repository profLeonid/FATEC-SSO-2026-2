# 🐳 Laboratório: Contêineres com Docker

Bem-vindo(a) ao guia prático da nossa aula de **Docker**! 
Este repositório contém todos os comandos e passos que utilizaremos durante a aula para aprender a rodar e gerenciar contêineres usando imagens prontas.

---

## 📋 Pré-requisitos
* **Docker Desktop** (Windows/Mac) ou **Docker Engine** (Linux) instalado e rodando.
* **Git** instalado.
* Um editor de código (ex: **VS Code**).

---

## 🚀 Primeiros Passos

### 1.1 Verificando o ambiente
Abra o seu terminal e rode os comandos abaixo para confirmar que o Docker está funcionando:

```bash
docker --version
docker run hello-world
```
> Todos os comandos devem retornar informações sem erros. Se houver algum problema, verifique a instalação do Docker.

### 1.2 Verificando imagens e contêineres
```bash
docker images
docker ps
```

### 1.2 Subindo o primeiro servidor web (Apache)
Vamos baixar e executar a imagem leve do Apache (`httpd:alpine`) em segundo plano:

```bash
docker run -d -p 8080:80 --name meu-apache httpd:alpine
```

> **Entendendo o comando:**
> * `docker run`: Cria e inicia um contêiner.
> * `-d`: Roda em segundo plano (*detached*), liberando o terminal.
> * `-p 8080:80`: Mapeia a porta `8080` do seu computador para a porta `80` do contêiner.
> * `--name meu-apache`: Define um nome amigável para o contêiner.
> * `httpd:alpine`: Nome da imagem oficial do Apache e sua versão ultraleve (`alpine`).

Abra o navegador e acesse: **`http://localhost:8080`**

### 1.3 Gerenciando o ciclo de vida do contêiner
Pratique os comandos básicos de controle:

```bash
# Listar contêineres ativos
docker ps

# Ver os logs de acesso do servidor
docker logs meu-apache

# Parar o contêiner
docker stop meu-apache

# Iniciar novamente
docker start meu-apache

# Remover o contêiner forçadamente
docker rm -f meu-apache
```

---

## 📂 Módulo 2: Rodando um Projeto do GitHub (Volumes)

Nesta etapa, vamos conectar uma pasta real do seu computador diretamente dentro do contêiner usando **Volumes (`-v`)**.

### 2.1 Preparando o projeto
Clone o projeto de exemplo para uma pasta na sua máquina (ou crie uma pasta com um arquivo `index.html`):

* **Mac / Linux:**
  ```bash
  git clone https://github.com ~/meu-site
  ```
* **Windows (CMD / Git Bash):**
  ```bash
  git clone https://github.com C:/meu-site
  ```

### 2.2 Conectando a pasta ao Apache (`-v`)
Suba o contêiner apontando a sua pasta local para a pasta padrão do Apache (`/usr/local/apache2/htdocs/`):

* **Mac / Linux:**
  ```bash
  docker run -d -p 8080:80 --name site-github -v ~/meu-site:/usr/local/apache2/htdocs/ httpd:alpine
  ```
* **Windows (CMD / Git Bash):**
  ```bash
  docker run -d -p 8080:80 --name site-github -v "C:/meu-site:/usr/local/apache2/htdocs/" httpd:alpine
  ```

Acesse **`http://localhost:8080`** no navegador. 

> 💡 **Dica:** Se a página não atualizar, force a limpeza do cache do navegador com **`Ctrl + Shift + R`** (Windows/Linux) ou **`Cmd + Shift + R`** (Mac).

### 2.3 Teste ao vivo e acesso interno
1. Abra o arquivo `index.html` no VS Code, altere qualquer texto, salve e atualize o navegador.
2. Entre no terminal interno do contêiner para inspecionar os arquivos:

```bash
docker exec -it site-github sh
ls /usr/local/apache2/htdocs/
exit
```

---

## 🎼 Módulo 3: Orquestrando Múltiplos Serviços (Docker Compose)

No dia a dia, aplicações precisam de outros serviços (como bancos de dados). O **Docker Compose** permite subir tudo com um único comando.

### 3.1 Criando o arquivo `docker-compose.yml`
Na pasta do seu projeto, crie um arquivo chamado **`docker-compose.yml`**:

```yaml
services:
  web:
    image: httpd:alpine
    ports:
      - "8080:80"
    volumes:
      - ./:/usr/local/apache2/htdocs/

  banco:
    image: postgres:alpine
    environment:
      POSTGRES_PASSWORD: 123
```

### 3.2 Subindo e derrubando o ambiente
No terminal, dentro da pasta onde está o `docker-compose.yml`, execute:

```bash
# Limpa o contêiner anterior para liberar a porta 8080
docker rm -f site-github

# Sobe o site e o banco de dados juntos
docker compose up -d

# Verifica os serviços rodando
docker compose ps

# Para e remove todos os contêineres e redes criados
docker compose down
```

---

## 🧹 Módulo 4: Limpeza Geral

Ao finalizar os estudos, você pode liberar espaço em disco removendo contêineres parados, redes e imagens não utilizadas:

```bash
docker system prune -a
```

---

## 📌 Resumo Rápido (Cheatsheet)

| Ação | Comando |
| :--- | :--- |
| Baixar e rodar contêiner | `docker run -d -p 8080:80 --name nome imagem` |
| Listar contêineres ativos | `docker ps` |
| Listar todos os contêineres | `docker ps -a` |
| Ver logs | `docker logs nome` |
| Entrar no terminal do contêiner | `docker exec -it nome sh` |
| Remover contêiner | `docker rm -f nome` |
| Subir Docker Compose | `docker compose up -d` |
| Derrubar Docker Compose | `docker compose down` |
