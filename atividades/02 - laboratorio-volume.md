# Laboratório: Volumes e Docker Compose

Nesta aula, vamos aprender a compartilhar arquivos do Windows com o contêiner usando **Volumes** e a gerenciar múltiplos serviços de forma organizada utilizando o **Docker Compose**.

---

## 📂 Conectando Arquivos Locais (Volumes)

Antes de ir para o Docker Compose, vamos entender como colocar um projeto real (como um site do GitHub) dentro do Apache sem precisar criar uma imagem nova.

### 1. Preparando a pasta do projeto
No **Git Bash**, crie uma pasta simples na raiz do disco `C:` e coloque um arquivo `index.html` dentro dela:

```bash
mkdir C:/meu-site
echo "<h1>Meu Site no Docker!</h1> <h2>nome-do-aluno</h2>" > C:/meu-site/index.html
```

### 2. Subindo o Apache com Volume (`-v`)
Vamos mapear a pasta `C:/meu-site` do seu Windows para a pasta padrão onde o Apache lê os arquivos do site (`/usr/local/apache2/htdocs/`):

```bash
docker run -d -p 8080:80 --name site-volume -v "C:/meu-site:/usr/local/apache2/htdocs/" httpd:alpine
```

> **Entendendo a flag `-v` (Volume):**
> * Funciona no formato `"PASTA_DO_WINDOWS:PASTA_DO_CONTEINER"`.
> * Tudo o que você editar na pasta `C:/meu-site` reflete **instantaneamente** dentro do contêiner.

Acesse **`http://localhost:8080`** no navegador. 
*(Dica: Se a página não atualizar de primeira, aperte `Ctrl + Shift + R` para limpar o cache).*

### 3. Alterando o conteúdo do site
Abra o arquivo `C:/meu-site/index.html` no seu editor de código e faça alguma alteração visível, como mudar o texto ou a cor do fundo. Salve o arquivo e atualize a página no navegador para ver a mudança refletida.

### 4. Limpando o ambiente para o próximo passo
Antes de avançar, vamos remover esse contêiner para liberar a porta `8080`:

```bash
docker rm -f site-volume
```

### 5. Exercício: Testando o Volume
1. Baixe um projeto HTML do GitHub e coloque em uma pasta local.
2. Suba um container do Apache mapeando essa pasta.
3. Acesse o site no navegador.
4. Faça alguma ateração bem visível no site.
5. Atualize a página no navegador e veja a mudança refletida.
6. Documente o passo a passo e tire prints da tela para evidenciar seu exercício, lembrando de sempre personalizar o conteúdo do site e nomes como as pastas do projeto, nome do contêiner, etc.

---