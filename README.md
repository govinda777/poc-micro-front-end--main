# POC Micro Front-end

Este é um projeto de prova de conceito (POC) para implementação de uma arquitetura de Micro Front-ends utilizando React.

## Visão Geral

O projeto demonstra uma implementação de Micro Front-ends com um aplicativo principal (main) que orquestra múltiplos aplicativos auxiliares (slaves). Esta arquitetura permite o desenvolvimento independente de diferentes partes da aplicação, mantendo a coesão e facilitando a manutenção.

## Estrutura e Arquitetura de Cada Projeto

### Aplicativo Principal: poc-micro-front-end--main

#### Responsabilidades:
- Gerenciamento de autenticação centralizada.
- Orquestração dos Micro Front-ends.
- Gerenciamento de estado global.
- Roteamento principal.
- Interface base, incluindo menus e layout.

#### Componentes Principais:
```
/src
├── /auth
│   ├── AuthProvider.tsx       # Contexto de autenticação.
│   ├── AuthGuard.tsx          # Proteção de rotas.
│   └── useAuth.tsx            # Hook de autenticação.
│
├── /core
│   ├── EventBus.ts            # Sistema de eventos.
│   ├── StateManager.ts        # Gerenciador de estado.
│   └── ModuleFederation.ts    # Configuração de Module Federation.
│
├── /layout
│   ├── MainLayout.tsx         # Layout principal.
│   ├── Header.tsx             # Cabeçalho com comboboxes.
│   └── Sidebar.tsx            # Menu lateral.
│
└── /shared
    ├── /components            # Componentes compartilhados.
    ├── /hooks                 # Hooks customizados.
    └── /utils                 # Utilitários.
```

#### Funcionalidades:
- **Autenticação**: Gerenciamento de login, tokens e controle de sessão.
- **Interface Principal**: Cabeçalho com comboboxes, menu lateral e área de conteúdo dinâmico.
- **Gerenciamento de Micro Front-ends**: Carregamento dinâmico e comunicação via sistema de eventos.
- **Gestão de Dependências**: Bibliotecas compartilhadas e resolução de conflitos de versões.

### Micro Front-ends: poc-micro-front-end--slave1 e poc-micro-front-end--slave2

#### Estrutura Comum:
```
/src
├── /components             # Componentes específicos do Micro Front-end.
├── /hooks                  # Hooks específicos.
├── /services               # Serviços específicos.
└── bootstrap.tsx           # Ponto de entrada.
```

#### Características:
- Integração com o sistema de autenticação e sessão do aplicativo principal.
- Consumo de eventos emitidos pelo aplicativo principal.
- Independência no desenvolvimento e deploy.

### Camada Proxy: poc-micro-front-end--proxy

#### Responsabilidades:
- Roteamento de requisições.
- Cache e balanceamento de carga.
- Autenticação e interceptadores de erros.

#### Estrutura do Proxy:
```
/proxy
├── /adapters                # Adaptadores para APIs.
├── /cache                   # Gerenciamento de cache.
├── /interceptors            # Interceptadores de requisição.
├── /queue                   # Sistema de filas.
└── /error-handling          # Tratamento de erros.
```

#### Componentes:
1. **Gateway API**: Responsável pelo roteamento e log centralizado.
2. **Gerenciamento de Cache**: Implementação de estratégias como `in-memory` e Redis.
3. **Interceptadores**: Manipula cabeçalhos, tokens e autenticação.
4. **Circuit Breaker**: Gerencia falhas e previne sobrecarga.

### Backend Layer (Camada de Backend)
- APIs REST independentes.
- Comunicação via proxy.
- Gerenciamento de autenticação e dados.

## Fluxos de Comunicação

1. **Autenticação**: O container principal gerencia tokens e distribui para os Micro Front-ends.
2. **Dados**: O proxy verifica cache antes de encaminhar para o backend.
3. **Comunicação Entre Módulos**: Utiliza um sistema de eventos baseado em `EventBus` para sincronização.

---

## Diagrama Geral da Arquitetura
```mermaid
graph TD
    MainApp[Main Application]
    Proxy[Proxy Layer]
    Backend[Backend Layer]

    MainApp --> Proxy --> Backend
    MainApp --> Slave1[Micro Front-end 1]
    MainApp --> Slave2[Micro Front-end 2]
```

## Desenvolvimento

1. Clone o repositório:
```bash
git clone https://github.com/seu-usuario/poc-micro-front-end.git
```

2. Instale as dependências:
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
# Em terminais separados
cd poc-micro-front-end--main
npm start

cd ../poc-micro-front-end--proxy
npm start

cd ../poc-micro-front-end--slave1
npm start

cd ../poc-micro-front-end--slave2
npm start
```


---

## Diagrams

> MF Main arq

```mermaid
graph TD
  %% Container Principal - Module Federation
  subgraph "Container Principal - Module Federation"
    A[Module Federation Manager]
    A --> B[Remote Entry 1]
    A --> C[Remote Entry 2]
    A --> D[Shared Dependencies]
  end

  %% UI Components
  subgraph "UI Components"
    E[Main Layout]
    E --> F[Header]
    E --> G[Sidebar]
    E --> H[Content Area]
    F --> I[Combo Box 1]
    F --> J[Combo Box 2]
    G --> K[Menu Items]
    H --> L[MFE Container]
  end

  %% Core Components
  subgraph "Core Components"
    %% Event Bus Details
    subgraph "Event Bus Details"
      M[Event Bus]
      M --> N[Event Registry]
      M --> O[Event Queue]
      M --> P[Event Emitter]
    end

    %% State Manager Details
    subgraph "State Manager Details"
      Q[State Manager]
      Q --> R[Global State]
      Q --> S[State Sync]
      Q --> T[State Context]
    end

    %% Auth Manager Details
    subgraph "Auth Manager Details"
      U[Authentication Manager]
      U --> V[Token Manager]
      U --> W[Session Storage]
      U --> X[Auth Context]
      U --> Y[Router]
    end
  end
```

> MF solution macro

```mermaid
flowchart TD
 subgraph subGraph0["Frontend Layer"]
        C["Container Principal"]
        B["Main Application"]
        D["Auth Manager"]
        E["Router"]
        F["State Manager"]
        G["Event Bus"]
        H["Micro Frontends"]
        I["Slave 1"]
        J["Slave 2"]
  end
 subgraph subGraph1["Proxy Layer"]
        L["Cache Manager"]
        K["API Gateway/Proxy"]
        M["Auth Interceptor"]
        N["Circuit Breaker"]
        O["Request Queue"]
  end
 subgraph subGraph2["Backend Layer"]
        P["API Service 1"]
        Q["API Service 2"]
        R["API Service 3"]
  end
    A["Browser/Cliente"] --> B
    B --> C & K
    C --> D & E & F & G & H
    H --> I & J
    K --> L & M & N & O & P & Q & R
    F -- Comunicação --> G
    G -- Eventos --> I & J
    subGraph0 --> n1["Untitled Node"]
```




