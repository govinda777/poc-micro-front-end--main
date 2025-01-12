# POC Micro Front-end

Este é um projeto de prova de conceito (POC) para implementação de uma arquitetura de Micro Front-ends utilizando React.

## Visão Geral

O projeto demonstra uma implementação de Micro Front-ends com um aplicativo principal (main) que orquestra múltiplos aplicativos auxiliares (slaves). Esta arquitetura permite o desenvolvimento independente de diferentes partes da aplicação, mantendo a coesão e facilitando a manutenção.

## Estrutura do Projeto

### Projetos:
- **poc-micro-front-end--main** (Aplicação Principal)
- **poc-micro-front-end--proxy** (Camada Proxy)
- **poc-micro-front-end--slave1** (Micro Front-end 1)
- **poc-micro-front-end--slave2** (Micro Front-end 2)

### Aplicação Principal: poc-micro-front-end--main

Responsável pela orquestração e gerenciamento dos Micro Front-ends.

#### Funcionalidades Principais:

- **Autenticação**
  - Gerenciamento completo do processo de autenticação.
  - Controle de sessão do usuário.

- **Interface Principal**
  - Menu superior com 2 Combo boxes.
  - Menu lateral esquerdo para navegação entre Micro Front-ends.
  - Área de conteúdo dinâmico para carregamento dos Micro Front-ends.

- **Gerenciamento de Micro Front-ends**
  - Orquestração do carregamento dinâmico.
  - Gerenciamento de dados necessários para cada Micro Front-end.
  - Sistema de eventos para comunicação com os Micro Front-ends.
  - Controle de versões de dependências.

- **Gestão de Dependências**
  - Gerenciamento centralizado de bibliotecas compartilhadas.
  - Sistema de resolução de conflitos de versões.
  - Mecanismo para lidar com incompatibilidades entre versões.

### Camada Proxy: poc-micro-front-end--proxy

Atua como middleware entre o frontend e o backend, com funcionalidades como:
- Roteamento de requisições.
- Cache e balanceamento de carga.
- Tratamento de erros e interceptadores de autenticação.

### Aplicações Auxiliares: poc-micro-front-end--slave1 e poc-micro-front-end--slave2

#### Características:
- Acesso aos dados de autenticação.
- Consumo dos dados de sessão do usuário.
- Integração com o sistema de eventos da aplicação principal.
- Independência no desenvolvimento e deploy.

---

## Comunicação entre Módulos

### Eventos do Sistema

A aplicação principal emite eventos para os Micro Front-ends nos seguintes casos:
- Alterações nos Combo boxes do menu superior.
- Atualizações no estado de autenticação.
- Modificações nos dados de sessão.

### Fluxo de Dados

```
Main App
  ├── Gerenciamento de Autenticação
  ├── Controle de Sessão
  ├── Sistema de Eventos
  └── Micro Front-ends
      ├── Slave 1
      └── Slave 2
```

---

## Requisitos Técnicos

- Node.js (versão LTS mais recente).
- React.
- Gerenciador de pacotes (npm/yarn).

## Como Executar

1. Clone o repositório:
   ```bash
   git clone https://github.com/seu-usuario/poc-micro-front-end.git
   ```

2. Instale as dependências em cada projeto:
   ```bash
   cd poc-micro-front-end--main
   npm install

   cd ../poc-micro-front-end--proxy
   npm install

   cd ../poc-micro-front-end--slave1
   npm install

   cd ../poc-micro-front-end--slave2
   npm install
   ```

3. Execute os projetos:
   ```bash
   # Em terminais separados:
   cd poc-micro-front-end--main
   npm start

   cd poc-micro-front-end--proxy
   npm start

   cd ../poc-micro-front-end--slave1
   npm start

   cd ../poc-micro-front-end--slave2
   npm start
   ```

## Processo de CI/CD

### Overview

Para gerenciar e automatizar o deploy das aplicações, utilizaremos **GitHub Actions** para construção e publicação das aplicações e **GitHub Pages** para hospedagem. O pipeline está configurado para:

1. Executar testes automatizados em cada commit realizado no repositório.
2. Construir os artefatos de produção (build).
3. Publicar os artefatos no GitHub Pages para disponibilização online.

### Configuração do GitHub Actions

Cada repositório do projeto possui um arquivo `workflow` no diretório `.github/workflows`. A seguir está um exemplo de configuração para o **poc-micro-front-end--main**:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 'lts/*'

      - name: Install Dependencies
        run: npm install

      - name: Build Project
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./build
```

### Deploy das Aplicações Auxiliares

Os workflows para **slave1** e **slave2** seguem a mesma lógica, ajustando o diretório de publicação e os comandos de build conforme necessário. Certifique-se de criar as páginas individuais para cada aplicação no GitHub Pages.

### Estrutura de Hospedagem

- **Main App**: Hospedada na raiz do repositório do GitHub Pages.
- **Slave 1**: Hospedado em um subdiretório, como `/slave1`.
- **Slave 2**: Hospedado em um subdiretório, como `/slave2`.

### Considerações

- Configure corretamente o `homepage` no `package.json` de cada projeto para refletir o caminho de publicaç

