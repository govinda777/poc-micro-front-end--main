# POC Micro Front-end

Este é um projeto de prova de conceito (POC) para implementação de uma arquitetura de Micro Front-ends utilizando React.

## Visão Geral

O projeto demonstra uma implementação de Micro Front-ends com um aplicativo principal (main) que orquestra múltiplos aplicativos escravos (slaves). Esta arquitetura permite o desenvolvimento independente de diferentes partes da aplicação, mantendo a coesão e facilitando a manutenção.

## Estrutura do Projeto

### poc-micro-front-end--main (Aplicação Principal)

Aplicação responsável pela orquestração e gerenciamento dos Micro Front-ends.

#### Funcionalidades Principais:

- **Autenticação**
  - Gerenciamento completo do processo de autenticação
  - Controle de sessão do usuário

- **Interface Principal**
  - Menu superior com 2 Combo boxes
  - Menu lateral esquerdo para navegação entre Micro Front-ends
  - Área de conteúdo dinâmico para carregamento dos Micro Front-ends

- **Gerenciamento de Micro Front-ends**
  - Orquestração do carregamento dinâmico
  - Gerenciamento de dados necessários para cada Micro Front-end
  - Sistema de eventos para comunicação com os Micro Front-ends
  - Controle de versões de dependências

- **Gestão de Dependências**
  - Gerenciamento centralizado de bibliotecas compartilhadas
  - Sistema de resolução de conflitos de versões
  - Mecanismo para lidar com incompatibilidades entre versões

### poc-micro-front-end--slave1 e poc-micro-front-end--slave2 (Aplicações Escravas)

Micro Front-ends que são carregados dinamicamente pela aplicação principal.

#### Características:

- Acesso aos dados de autenticação
- Consumo dos dados de sessão do usuário
- Integração com o sistema de eventos da aplicação principal
- Independência no desenvolvimento e deploy

## Comunicação entre Módulos

### Eventos do Sistema

A aplicação principal emite eventos para os Micro Front-ends nos seguintes casos:
- Alterações nos Combo boxes do menu superior
- Atualizações no estado de autenticação
- Modificações nos dados de sessão

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

## Requisitos Técnicos

- Node.js (versão LTS mais recente)
- React
- Gerenciador de pacotes (npm/yarn)

## Como Executar

1. Clone o repositório
```bash
git clone https://github.com/seu-usuario/poc-micro-front-end.git
```

2. Instale as dependências em cada projeto
```bash
cd poc-micro-front-end--main
npm install

cd ../poc-micro-front-end--slave1
npm install

cd ../poc-micro-front-end--slave2
npm install
```

3. Execute os projetos
```bash
# Em terminais separados, execute:
# Terminal 1
cd poc-micro-front-end--main
npm start

# Terminal 2
cd poc-micro-front-end--slave1
npm start

# Terminal 3
cd poc-micro-front-end--slave2
npm start
```

## Desenvolvimento

### Boas Práticas

- Mantenha as dependências atualizadas
- Siga os padrões de código estabelecidos
- Documente todas as interfaces de comunicação entre os módulos
- Realize testes adequados antes de integrar novos Micro Front-ends

### Versionamento

Utilize semantic versioning (SemVer) para controle de versões dos Micro Front-ends.

## Contribuição

1. Faça o fork do projeto
2. Crie sua feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.
