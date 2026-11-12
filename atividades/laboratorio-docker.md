# 🐳 Laboratório: Contêineres com Docker

Bem-vindo(a) ao guia prático da nossa aula de **Docker**! 
Este repositório contém todos os comandos e passos que utilizaremos durante a aula para aprender a rodar e gerenciar contêineres usando imagens prontas.

---

## 📋 Pré-requisitos
* **Docker Desktop** (Windows/Mac) ou **Docker Engine** (Linux) instalado e rodando.
* **Git** instalado.
* Um editor de código (ex: **VS Code**).

---

## 🚀 Primeiros Passos e apache

### 1.1 Verificando o ambiente
Abra o seu terminal e rode os comandos abaixo para confirmar que o Docker está funcionando:

```bash
# Verificando se o WSL2 está ativo (Windows)
wsl --list --verbose

# Verificando se o Docker está instalado
docker --version

# Rodando o contêiner de teste "Hello World"
docker run hello-world
```
> Todos os comandos devem retornar informações sem erros. Se houver algum problema, verifique a instalação do Docker.

### 1.2 Verificando imagens e contêineres
```bash
docker images
docker ps
```

### 1.3 Subindo o primeiro servidor web (Apache)
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

### 1.4 Gerenciando o ciclo de vida do contêiner
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

# Remover todos os contêineres parados forçadamente
docker container prune -f

# Remover todos os contêineres, inclusive os ativos (cuidado!)
docker rm -f $(docker ps -aq)
```

---