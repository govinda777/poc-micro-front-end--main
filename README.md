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

---

## Implementação

# Arquitetura e Plano de Implementação - POC Micro Front-end

## 1. Visão Geral da Arquitetura

A arquitetura do sistema é dividida em três camadas principais que trabalham em conjunto para fornecer uma experiência de usuário coesa e performática:

### 1.1 Camada de Frontend (Frontend Layer)

#### Container Principal (Main Application)
- **Responsabilidades**:
  - Gerenciamento de autenticação centralizada
  - Orquestração dos Micro Front-ends
  - Gestão de estado global
  - Roteamento principal
  - Interface base (menus e layout)

- **Componentes Principais**:
  ```
  /src
  ├── /auth
  │   ├── AuthProvider.tsx       # Contexto de autenticação
  │   ├── AuthGuard.tsx         # Proteção de rotas
  │   └── useAuth.tsx           # Hook de autenticação
  │
  ├── /core
  │   ├── EventBus.ts           # Sistema de eventos
  │   ├── StateManager.ts       # Gerenciador de estado
  │   └── ModuleFederation.ts   # Configuração de Module Federation
  │
  ├── /layout
  │   ├── MainLayout.tsx        # Layout principal
  │   ├── Header.tsx           # Cabeçalho com combos
  │   └── Sidebar.tsx          # Menu lateral
  │
  └── /shared
      ├── /components          # Componentes compartilhados
      ├── /hooks              # Hooks customizados
      └── /utils              # Utilitários
  ```

#### Micro Front-ends (Slaves)
- **Estrutura Comum**:
  ```
  /src
  ├── /components             # Componentes específicos
  ├── /hooks                 # Hooks específicos
  ├── /services             # Serviços específicos
  └── bootstrap.tsx         # Ponto de entrada
  ```

### 1.2 Camada de Proxy (Proxy Layer)

O proxy atua como um middleware entre o frontend e o backend, proporcionando:

#### Estrutura do Proxy
```
/proxy
├── /adapters                # Adaptadores para APIs
├── /cache                   # Gerenciamento de cache
├── /interceptors           # Interceptadores de requisição
├── /queue                  # Sistema de filas
└── /error-handling        # Tratamento de erros
```

#### Componentes do Proxy

1. **Gateway API**
   - Roteamento de requisições
   - Load balancing
   - Rate limiting
   - Logging centralizado

2. **Cache Manager**
   ```typescript
   interface CacheConfig {
     ttl: number;
     strategy: 'memory' | 'redis';
     invalidationRules: InvalidationRule[];
   }
   
   class CacheManager {
     set(key: string, value: any, config: CacheConfig): void;
     get(key: string): Promise<any>;
     invalidate(pattern: string): void;
   }
   ```

3. **Auth Interceptor**
   ```typescript
   class AuthInterceptor {
     intercept(request: Request): Promise<Response> {
       // Adiciona tokens
       // Verifica autenticação
       // Gerencia refresh tokens
     }
   }
   ```

4. **Circuit Breaker**
   ```typescript
   interface CircuitBreakerConfig {
     failureThreshold: number;
     resetTimeout: number;
     fallbackResponse?: any;
   }
   
   class CircuitBreaker {
     execute(request: Request): Promise<Response>;
     // Monitora falhas
     // Previne sobrecarga
   }
   ```

### 1.3 Camada de Backend (Backend Layer)

- Múltiplas APIs REST
- Cada API é independente
- Comunicação via proxy

## 2. Fluxos de Comunicação

### 2.1 Fluxo de Autenticação
1. Usuário acessa aplicação
2. Container principal solicita login
3. Proxy intercepta e adiciona tokens
4. Backend valida e retorna sessão
5. Container principal distribui informações

### 2.2 Fluxo de Dados
1. Micro Frontend solicita dados
2. Requisição passa pelo proxy
3. Proxy verifica cache
4. Se necessário, encaminha para backend
5. Resposta é cacheada
6. Dados retornam ao Micro Frontend

### 2.3 Comunicação Entre Micro Front-ends
```typescript
// Event Bus
interface EventBus {
  emit(event: string, payload: any): void;
  on(event: string, callback: (payload: any) => void): void;
  off(event: string, callback: (payload: any) => void): void;
}

// Usage
eventBus.emit('COMBO_CHANGED', { id: 1, value: 'new' });

// In Micro Frontend
eventBus.on('COMBO_CHANGED', (payload) => {
  // Handle change
});
```

## 3. Implementação por Fases

### Fase 1: Fundação (4 semanas)
- Setup do Container Principal
- Implementação do Proxy Básico
- Sistema de Autenticação

### Fase 2: Core Features (5 semanas)
- Module Federation
- Sistema de Eventos
- Cache e Circuit Breaker
- Gerenciamento de Estado

### Fase 3: Micro Front-ends (3 semanas)
- Implementação dos Slaves
- Integração com Proxy
- Testes de Comunicação

### Fase 4: Refinamento (3 semanas)
- Otimização de Performance
- Testes E2E
- Documentação
- Monitoring

## 4. Considerações de Performance

### 4.1 Estratégias de Cache
```typescript
const cacheConfig: CacheConfig = {
  ttl: 3600,
  strategy: 'redis',
  invalidationRules: [
    {
      pattern: 'user.*',
      events: ['USER_UPDATED', 'USER_DELETED']
    }
  ]
};
```

### 4.2 Code Splitting
```javascript
// webpack.config.js
module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'main',
      remotes: {
        slave1: 'slave1@http://localhost:3001/remoteEntry.js',
        slave2: 'slave2@http://localhost:3002/remoteEntry.js'
      },
      shared: ['react', 'react-dom']
    })
  ]
};
```

## 5. Monitoramento e Métricas

### 5.1 Métricas do Frontend
- Tempo de carregamento
- Performance de renderização
- Erros de JavaScript
- Usage analytics

### 5.2 Métricas do Proxy
- Latência
- Taxa de cache hit/miss
- Estado do circuit breaker
- Erros de comunicação

## 6. Escalabilidade

### 6.1 Horizontal Scaling
- Load balancing no proxy
- Múltiplas instâncias de cache
- Distribuição geográfica

### 6.2 Vertical Scaling
- Otimização de bundles
- Lazy loading de módulos
- Preload de recursos críticos

## 7. Segurança

### 7.1 Autenticação
- JWT com refresh tokens
- Rate limiting
- CORS configurado
- XSS protection

### 7.2 Comunicação
- HTTPS em todas as camadas
- Sanitização de dados
- Validação de inputs
- Auditoria de acessos

## 8. Development Workflow

### 8.1 Local Development
```bash
# Terminal 1 - Main
cd main && npm start

# Terminal 2 - Proxy
cd proxy && npm start

# Terminal 3 - Slave 1
cd slave1 && npm start

# Terminal 4 - Slave 2
cd slave2 && npm start
```

### 8.2 Build e Deploy
```bash
# Build
npm run build

# Deploy
docker-compose up -d
```

## 9. Documentação

### 9.1 API Documentation
- Swagger/OpenAPI para endpoints
- JSDoc para componentes
- Storybook para UI

### 9.2 Architecture Documentation
- Diagramas de sequência
- Fluxos de dados
- Guias de integração

## Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.
