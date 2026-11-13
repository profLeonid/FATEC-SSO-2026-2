# Projeto Integrado: Sistema de Gestão de Catálogo e Estoque de Delivery
**FATEC São Roque — Disciplinas: Servidores e Sistemas Operacionais (SSO) & Programação de Sítios Internet (PSI)**

---

## 📋 Contexto e Objetivo
Este repositório contém as diretrizes, infraestrutura e especificações para o desenvolvimento do projeto prático integrado das disciplinas de SSO e PSI. O objetivo é desenvolver um sistema web de **Gestão de Catálogo e Estoque de Delivery**, sustentado por uma infraestrutura de rede e servidores corporativos locais, consumindo uma API externa hospedada em nuvem.

---

## 🌐 Arquitetura de Rede e Infraestrutura (SSO)

### O que deve ser feito / Implementado
* **Windows Server (Rede Interna / Host-Only):**
  * Configurar o servidor para prover os serviços de rede essenciais.
  * Implantar os serviços de **DHCP** e **DNS** (utilizando domínio local `.empresa.p2.br`).
  * Configurar o **IIS** para hospedagem local e testes de serviços.
* **Windows Cliente (Ambiente de Testes com 2 Placas de Rede):**
  * **Placa 1 (Rede Interna):** Conectada ao Windows Server para obter IP via DHCP, resolver nomes no DNS local (`.empresa.p2.br`) e acessar os recursos internos.
  * **Placa 2 (Modo NAT / Bridge):** Conectada à internet para permitir o acesso externo e o consumo da API hospedada no Render.

### Checklist de Validação — SSO
* [ ] O Windows Server está configurado corretamente com IP estático e serviços de DHCP e DNS (`.empresa.p2.br`) ativos?
* [ ] O Windows Cliente possui as duas placas configuradas (uma na rede interna e outra em NAT)?
* [ ] O cliente obtém IP automaticamente do servidor e resolve os nomes de domínio interno?
* [ ] O cliente consegue acessar a internet e consumir os dados da API no Render através do navegador?
* [ ] O serviço IIS está configurado e acessível na rede interna?

---

## 💻 Especificações e Desenvolvimento — Programação de Sítios Internet (PSI)

### O que deve ser feito / Implementado
* **Tecnologias e Arquitetura Front-end:**
  * Desenvolver a interface utilizando estritamente **HTML, CSS e JavaScript puro**, garantindo uma aplicação limpa, funcional e utilizável.
  * Consumir a API feita com json-server e hospedada no **Render**. ([Container - json-server](https://github.com/fernandoleonid/container-json-server))
* **Regras de Negócio e Funcionalidades:**
  * **Nicho do Delivery:** Adaptar o sistema para o segmento escolhido (ex: pizzaria, adega, lanchonete) com campos e regras específicas.
  * **Vitrine e Tela de Detalhes:** Criar a listagem de produtos com status de estoque e uma tela/modal dedicada para exibir informações aprofundadas ao clicar em um item.
  * **Busca e Filtros:** Implementar pesquisa por texto e filtros por categorias específicas do segmento.
  * **Gerenciamento de Imagens:** Utilizar armazenamento em nuvem externa (como o **Cloudinary** ou similares) para gerenciar e exibir os links públicos das imagens.
  * **Painel Administrativo (CRUD e Controle de Estoque):** Desenvolver a interface para cadastrar, editar, excluir produtos e realizar o controle dinâmico da quantidade em estoque (entrada e baixa/atualização).
* **Práticas de Versionamento (Git):**
  * Realizar o versionamento contínuo do código-fonte no repositório do GitHub.
  * **Regra de Commits:** Fazer *commits* regulares a cada funcionalidade criada e a cada dia de aula. *(Atenção: Não será aceito um único commit final ou commits concentrados de última hora).*

### Checklist de Validação — PSI
* [ ] O sistema reflete o nicho de delivery escolhido com campos e regras específicas?
* [ ] A interface em HTML/CSS/JS puro está funcional e limpa?
* [ ] O Front-end consome corretamente a API no Render (leitura, criação, edição e exclusão)?
* [ ] O catálogo exibe os produtos e abre a tela de detalhes corretamente ao clicar em um item?
* [ ] O usuário consegue realizar buscas por texto e filtrar os produtos por categorias?
* [ ] As imagens dos produtos utilizam links externos hospedados em nuvem (ex: Cloudinary)?
* [ ] O painel administrativo permite gerenciar o CRUD de produtos?
* [ ] **Controle de Estoque:** O sistema gerencia, atualiza e exibe corretamente as quantidades e o status de disponibilidade do estoque na vitrine e no painel?
* [ ] **Histórico de Versionamento (Peso: 50% da nota):** O repositório apresenta *commits* regulares e distribuídos ao longo do desenvolvimento, refletindo a evolução contínua da aplicação?

---

## 📦 Como Entregar o Projeto

### Entrega — Servidores e Sistemas Operacionais (SSO)
* Documentar todo o passo a passo de configuração da infraestrutura e dos serviços (Rede, DHCP, DNS, IIS e testes de validação no Cliente).
* Incluir **prints de tela** comprovando cada etapa (sempre que possível personalizados com o nome da empresa/grupo ou usuário visível).
* Exportar a documentação completa em formato **PDF** e anexá-la na *Issue* pertinente do repositório.

### Entrega — Programação de Sítios Internet (PSI)
* Publicar o código-fonte completo do front-end no repositório do GitHub.
* Habilitar o **GitHub Pages** no repositório para que a aplicação fique acessível online.
* Enviar o link do repositório e o link do GitHub Pages na *Issue* pertinente.

---

## 📅 Prazo de Entrega
* **Data Limite:** Até o dia **27 de novembro**.