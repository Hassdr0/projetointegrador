# Documentação do Projeto CentroRecursos

## 1. Visão Geral do Projeto

**Nome**: Sistema Abrangente de Reserva e Empréstimo de Recursos (CentroRecursos)

**Objetivo**: Otimizar a gestão e o uso de recursos físicos em ambientes institucionais como universidades, escolas ou empresas através de uma plataforma web centralizada que elimina ineficiências nos processos manuais de reserva e empréstimo.

**Problema Resolvido**: A aplicação resolve a falta de transparência e as ineficiências nos processos manuais de reserva e empréstimo de ativos físicos (salas, equipamentos, materiais audiovisuais), oferecendo uma solução digital completa para gestão de recursos institucionais.

---

## 2. Tecnologias Utilizadas

### 2.1 Frontend

| Tecnologia | Versão | Função |
|------------|--------|--------|
| **React** | 18.3.1 | Framework JavaScript principal para construção da interface |
| **TypeScript** | 5.5.3 | Linguagem que adiciona tipagem estática ao JavaScript |
| **Vite** | 5.4.2 | Build tool e servidor de desenvolvimento de alta performance |
| **React Router DOM** | 7.7.1 | Gerenciamento de rotas e navegação SPA |
| **Tailwind CSS** | 3.4.1 | Framework CSS utility-first para estilização |
| **Lucide React** | 0.344.0 | Biblioteca de ícones modernos |
| **PostCSS** | 8.4.35 | Processador CSS para transformações |
| **Autoprefixer** | 10.4.18 | Plugin PostCSS para compatibilidade entre navegadores |

### 2.2 Backend-as-a-Service (BaaS)

| Tecnologia | Versão | Função |
|------------|--------|--------|
| **Supabase** | - | Plataforma BaaS completa (Backend-as-a-Service) |
| **PostgreSQL** | - | Banco de dados relacional gerenciado pelo Supabase |
| **Supabase Auth** | - | Sistema de autenticação e autorização |
| **Supabase Realtime** | - | Comunicação em tempo real via WebSockets |
| **@supabase/supabase-js** | 2.53.0 | Cliente JavaScript para integração com Supabase |

### 2.3 Ferramentas de Desenvolvimento

| Ferramenta | Versão | Função |
|------------|--------|--------|
| **ESLint** | 9.9.1 | Linter para análise estática de código |
| **TypeScript ESLint** | 8.3.0 | Plugin ESLint para TypeScript |
| **Vite Plugin React** | 4.3.1 | Plugin Vite para suporte a React |

---

## 3. Arquitetura da Solução

### 3.1 Padrão Arquitetural

O projeto utiliza uma arquitetura **Cliente-Servidor com Backend-as-a-Service (BaaS)**:

```
┌─────────────────────────────────────────┐
│     CLIENTE (Frontend - Browser)        │
├─────────────────────────────────────────┤
│  React SPA + TypeScript                 │
│  ├─ Componentes UI                      │
│  ├─ Context API (Estado Global)         │
│  ├─ React Router (Navegação)            │
│  └─ Camada de Serviços                  │
└──────────────┬──────────────────────────┘
               │ API REST / WebSockets
               ▼
┌─────────────────────────────────────────┐
│   BACKEND (Supabase BaaS - Cloud)       │
├─────────────────────────────────────────┤
│  Supabase Auth (Autenticação)           │
│  PostgreSQL (Banco de Dados)            │
│  Supabase Realtime (Chat/Notificações)  │
│  Row Level Security (RLS)               │
└─────────────────────────────────────────┘
```

### 3.2 Camadas da Aplicação

#### **Camada de Apresentação (Frontend)**
- Desenvolvida em React com TypeScript
- Estilização com Tailwind CSS
- Gerenciamento de estado com React Context API
- Roteamento com React Router DOM
- Comunicação com backend via `@supabase/supabase-js`

#### **Camada de Negócio (Lógica de Aplicação)**
- Validações no frontend (UX imediata)
- Regras de negócio implementadas através de:
  - Políticas RLS (Row Level Security) no PostgreSQL
  - Validações e verificações na camada de serviços
  - Funções de banco de dados (triggers, constraints)

#### **Camada de Dados (Backend)**
- PostgreSQL gerenciado pelo Supabase
- Autenticação com Supabase Auth
- APIs RESTful auto-geradas
- Realtime subscriptions via WebSockets

---

## 4. Funcionalidades Principais

### 4.1 Gestão de Usuários e Autenticação

**Características**:
- Sistema de autenticação com email e senha
- Controle de acesso baseado em papéis (RBAC)
- Três tipos de usuários: Estudante, Professor, Administrador
- Perfis de usuário com departamento e informações pessoais

**Tecnologia**: Supabase Auth + RLS Policies

### 4.2 Catálogo de Recursos

**Características**:
- Visualização de recursos disponíveis
- Busca e filtros por categoria (salas, equipamentos, audiovisual)
- Filtros por status (disponível, reservado, manutenção)
- Informações detalhadas: nome, descrição, localização, quantidade, especificações
- Imagens dos recursos

**Componentes**: `ResourceCatalog.tsx`, `ResourceManagement.tsx`

### 4.3 Sistema de Reservas

**Características**:
- Formulário de solicitação de reserva
- Validação de conflitos de horário
- Verificação de quantidade disponível
- Sistema de aprovação hierárquico
- Calendário visual de reservas
- Status de reservas (pendente, aprovada, rejeitada)

**Componentes**: `RequestForm.tsx`, `ReservationCalendar.tsx`, `ApprovalWorkflow.tsx`

**Fluxo**:
1. Usuário solicita reserva
2. Sistema verifica conflitos
3. Reserva fica pendente
4. Professor/Admin aprova ou rejeita
5. Usuário é notificado

### 4.4 Gerenciamento de Manutenção

**Características** (Apenas Administradores):
- Agendamento de tarefas de manutenção
- Tipos: rotina, reparo, inspeção, upgrade
- Status: agendada, em andamento, concluída, cancelada
- Prioridades e responsáveis
- Rastreamento de custos
- Integração com status dos recursos

**Componente**: `MaintenanceScheduler.tsx`

### 4.5 Chat em Tempo Real

**Características**:
- Comunicação entre usuários e administradores
- Mensagens instantâneas via WebSockets
- Estudantes/Professores conversam apenas com admins
- Admins podem conversar com todos

**Tecnologia**: Supabase Realtime + WebSockets

**Componentes**: `Chat.tsx`, `ChatContext.tsx`

### 4.6 Central de Notificações

**Características**:
- Alertas sobre aprovações de reserva
- Lembretes de reservas futuras
- Avisos de manutenção
- Status de leitura
- Notificações em tempo real

**Componente**: `NotificationCenter.tsx`

### 4.7 Relatórios e Analytics

**Características** (Apenas Administradores):
- Total de reservas e taxa de aprovação
- Tendências mensais de uso
- Utilização de recursos
- Estatísticas de aprovação
- Feed de atividades recentes

**Componente**: `UsageReports.tsx`

### 4.8 Gerenciamento de Usuários

**Características** (Apenas Administradores):
- Listagem de todos os usuários
- Edição de perfis e papéis
- Filtros por departamento e tipo
- Gerenciamento de permissões

**Componente**: `UserManagement.tsx`

### 4.9 Gerenciamento de Pacotes

**Características**:
- Criação de pacotes de recursos
- Agrupamento de múltiplos recursos
- Reserva simplificada de conjuntos

**Componente**: `PackageManagement.tsx`

### 4.10 Configurações do Sistema

**Características** (Apenas Administradores):
- Configurações gerais (nome, fuso horário)
- Configurações de notificações
- Limites de reserva
- Configurações de segurança (timeout, 2FA, auditoria)

**Componente**: `SystemSettings.tsx`

### 4.11 Dashboard com IA

**Características** (Apenas Administradores):
- Sugestões inteligentes baseadas em padrões de uso
- Análise preditiva de demanda
- Otimização de recursos

**Componente**: `AIDashboard.tsx`

---

## 5. Modelo de Dados

### 5.1 Tabelas Principais

#### **users**
- `id` (UUID, PK)
- `email` (text, unique)
- `name` (text)
- `role` (text: student, faculty, admin)
- `department` (text)
- `created_at`, `updated_at`

#### **resources**
- `id` (UUID, PK)
- `name` (text)
- `category` (text: rooms, equipment, av)
- `description` (text)
- `status` (text: available, reserved, maintenance)
- `location` (text)
- `image` (text)
- `quantity` (integer)
- `specifications` (jsonb)
- `created_at`, `updated_at`

#### **reservations**
- `id` (UUID, PK)
- `user_id` (UUID, FK → users)
- `resource_id` (UUID, FK → resources)
- `start_date`, `end_date` (timestamp)
- `purpose`, `description` (text)
- `status` (text: pending, approved, rejected)
- `priority` (text: low, normal, high, urgent)
- `attendees` (integer)
- `requirements` (text)
- `created_at`, `updated_at`

#### **maintenance_tasks**
- `id` (UUID, PK)
- `resource_id` (UUID, FK → resources)
- `type` (text: routine, repair, inspection, upgrade)
- `title`, `description` (text)
- `scheduled_date` (timestamp)
- `estimated_duration` (integer)
- `status` (text: scheduled, in-progress, completed, cancelled)
- `priority` (text: low, medium, high, critical)
- `assigned_to` (text)
- `cost` (numeric)
- `notes` (text)
- `created_at`, `updated_at`

#### **messages**
- `id` (UUID, PK)
- `sender_id` (UUID, FK → users)
- `receiver_id` (UUID, FK → users)
- `message_text` (text)
- `created_at` (timestamp)

### 5.2 Relacionamentos

```
users 1 ────── * reservations
users 1 ────── * messages (sender)
users 1 ────── * messages (receiver)
resources 1 ── * reservations
resources 1 ── * maintenance_tasks
```

---

## 6. Segurança e Controle de Acesso

### 6.1 Row Level Security (RLS)

O projeto implementa **RLS (Row Level Security)** em todas as tabelas do PostgreSQL:

#### **Políticas de Segurança**:

**users**:
- Usuários podem inserir próprio perfil
- Todos podem ler perfis (necessário para chat)
- Usuários podem atualizar apenas próprio perfil

**resources**:
- Todos autenticados podem visualizar
- Apenas admins podem gerenciar (CREATE, UPDATE, DELETE)

**reservations**:
- Usuários podem criar próprias reservas
- Usuários veem apenas suas reservas
- Admins/Professores veem todas as reservas
- Admins/Professores podem aprovar/rejeitar

**maintenance_tasks**:
- Todos autenticados podem visualizar
- Apenas admins podem gerenciar

**messages**:
- Usuários podem enviar mensagens
- Usuários veem apenas suas conversas
- Admins têm acesso a todas as mensagens

### 6.2 Controle de Acesso Baseado em Papéis (RBAC)

| Papel | Permissões |
|-------|-----------|
| **Estudante** | Visualizar catálogo, fazer reservas, ver próprias reservas, usar chat |
| **Professor** | Tudo do estudante + aprovar/rejeitar reservas, ver todas as reservas |
| **Administrador** | Acesso total: gerenciar recursos, usuários, manutenção, configurações, relatórios |

**Implementação**: `ProtectedRoute.tsx` com verificação de `requiredRole`

---

## 7. Gerenciamento de Estado

### 7.1 React Context API

O projeto utiliza três contextos principais:

#### **UserContext** (`context/UserContext.tsx`)
- Gerencia dados do usuário autenticado
- Controla papel e permissões
- Mantém sessão ativa

#### **ReservationContext** (`context/ReservationContext.tsx`)
- Gerencia estado de reservas
- Métodos para criar, atualizar, aprovar reservas
- Verificação de conflitos

#### **ChatContext** (`context/ChatContext.tsx`)
- Gerencia conversas e mensagens
- Conexão realtime com Supabase
- Estado de conversas ativas

---

## 8. Estrutura de Componentes

### 8.1 Componentes de Autenticação
- `LoginSelection.tsx` - Seleção de tipo de login
- `LoginSolicitante.tsx` - Login para estudantes/professores
- `LoginAdministrador.tsx` - Login para administradores
- `ProtectedRoute.tsx` - Proteção de rotas

### 8.2 Componentes de Navegação
- `Navbar.tsx` - Barra de navegação principal
- `Dashboard.tsx` - Dashboard principal

### 8.3 Componentes de Recursos
- `ResourceCatalog.tsx` - Catálogo de recursos
- `ResourceManagement.tsx` - Gerenciamento (Admin)
- `PackageManagement.tsx` - Gestão de pacotes

### 8.4 Componentes de Reservas
- `RequestForm.tsx` - Formulário de solicitação
- `ReservationCalendar.tsx` - Calendário visual
- `ApprovalWorkflow.tsx` - Fluxo de aprovação

### 8.5 Componentes de Manutenção
- `MaintenanceScheduler.tsx` - Agendamento de manutenção

### 8.6 Componentes de Comunicação
- `Chat.tsx` - Chat em tempo real
- `NotificationCenter.tsx` - Central de notificações

### 8.7 Componentes Administrativos
- `UserManagement.tsx` - Gestão de usuários
- `UsageReports.tsx` - Relatórios
- `SystemSettings.tsx` - Configurações
- `PasswordResetManagement.tsx` - Reset de senhas
- `AIDashboard.tsx` - Dashboard com IA

---

## 9. Soluções Técnicas Implementadas

### 9.1 Verificação de Conflitos de Reserva

**Problema**: Evitar dupla reserva do mesmo recurso no mesmo horário.

**Solução**:
- Query no banco verificando sobreposição de datas
- Considera quantidade disponível do recurso
- Validação antes de criar reserva

### 9.2 Comunicação em Tempo Real

**Problema**: Chat instantâneo e notificações.

**Solução**:
- Supabase Realtime via WebSockets
- Subscriptions em tempo real nas tabelas `messages`
- Auto-atualização da UI quando há novas mensagens

### 9.3 Gerenciamento de Sessão

**Problema**: Manter usuário autenticado.

**Solução**:
- Tokens JWT gerenciados pelo Supabase Auth
- Refresh automático de tokens
- Logout em caso de token expirado

### 9.4 Otimização de Performance

**Soluções Implementadas**:
- Vite para build rápido e HMR (Hot Module Replacement)
- Índices no banco de dados em colunas frequentemente consultadas
- Lazy loading de componentes com React Router
- Context API ao invés de prop drilling

### 9.5 Responsividade

**Solução**:
- Tailwind CSS com classes responsivas
- Design mobile-first
- Breakpoints para diferentes tamanhos de tela

---

## 10. Regras de Negócio Implementadas

1. **Email Único**: Cada usuário deve ter email institucional único
2. **Admin Padrão**: `miguel.oliveira@universidade.edu.br` é sempre admin
3. **Categorias Fixas**: Recursos em categorias predefinidas
4. **Status de Recursos**: Apenas disponível, reservado ou manutenção
5. **Validação de Datas**: Término deve ser após início
6. **Conflito de Agendamento**: Não permite sobreposição considerando quantidade
7. **Status Inicial**: Toda reserva inicia como "pendente"
8. **Aprovação Hierárquica**: Apenas professor/admin pode aprovar
9. **Chat Restrito**: Estudantes só conversam com admins
10. **Gerenciamento Restrito**: Apenas admins gerenciam recursos e manutenção

---

## 11. Fluxos Principais do Sistema

### 11.1 Fluxo de Reserva

```
1. Usuário acessa catálogo de recursos
2. Seleciona recurso desejado
3. Preenche formulário (datas, horário, propósito)
4. Sistema verifica conflitos
5. Se OK, cria reserva com status "pendente"
6. Notifica professor/admin
7. Professor/Admin aprova ou rejeita
8. Sistema notifica usuário solicitante
9. Se aprovado, recurso aparece no calendário
```

### 11.2 Fluxo de Manutenção

```
1. Admin identifica necessidade de manutenção
2. Acessa agendador de manutenção
3. Cria tarefa (tipo, data, responsável, custo)
4. Sistema atualiza status do recurso para "manutenção"
5. Recurso fica indisponível para reserva
6. Admin atualiza progresso (agendada → em andamento → concluída)
7. Ao concluir, recurso volta para "disponível"
```

### 11.3 Fluxo de Comunicação

```
1. Usuário abre chat
2. Seleciona admin para conversar
3. Envia mensagem
4. Mensagem salva no banco
5. Realtime subscription notifica destinatário
6. Admin responde
7. Mensagem aparece instantaneamente
```

---

## 12. Requisitos Não Funcionais Atendidos

| Requisito | Solução Implementada |
|-----------|---------------------|
| **Performance** | Vite, índices no BD, queries otimizadas |
| **Segurança** | RLS, RBAC, Supabase Auth, validações |
| **Usabilidade** | Tailwind CSS, UI intuitiva, feedback visual |
| **Confiabilidade** | Tratamento de erros, validações, constraints |
| **Escalabilidade** | Supabase gerenciado, arquitetura stateless |
| **Manutenibilidade** | TypeScript, código modular, Context API |
| **Compatibilidade** | Responsivo, suporte a navegadores modernos |

---

## 13. Diferenciais da Solução

1. **Backend-as-a-Service**: Reduz complexidade de infraestrutura
2. **Realtime**: Chat e notificações instantâneas
3. **Segurança Robusta**: RLS nativo do PostgreSQL
4. **TypeScript**: Código mais seguro e manutenível
5. **UI Moderna**: Design clean com Tailwind CSS
6. **Gerenciamento Completo**: Desde reserva até manutenção
7. **Multi-nível de Acesso**: RBAC com três papéis
8. **Analytics**: Relatórios de uso para tomada de decisão
9. **IA Integrada**: Sugestões inteligentes para otimização
10. **Escalável**: Arquitetura preparada para crescimento

---

## 14. Futuras Melhorias Sugeridas

1. **Notificações Push**: Implementar PWA com service workers
2. **Calendário Semanal/Diário**: Expandir visualizações do calendário
3. **Exportação de Relatórios**: PDF, Excel, CSV
4. **Integração com APIs Externas**: Google Calendar, Outlook
5. **QR Code**: Check-in de recursos via QR
6. **Mobile App**: Versão nativa para iOS e Android
7. **Pagamentos**: Integração com gateway de pagamento para recursos pagos
8. **Multi-idioma**: i18n para suporte a múltiplos idiomas
9. **Histórico Completo**: Auditoria detalhada de ações
10. **Machine Learning**: Previsão de demanda mais avançada

---

## 15. Como Executar o Projeto

### 15.1 Pré-requisitos
- Node.js 18+
- Conta no Supabase
- Git

### 15.2 Instalação

```bash
# Clonar repositório
git clone <repo-url>

# Instalar dependências
npm install

# Configurar variáveis de ambiente
cp .env.example .env
# Editar .env com credenciais do Supabase

# Executar migrações
# (Executar scripts SQL na pasta supabase/migrations no Supabase Dashboard)

# Iniciar servidor de desenvolvimento
npm run dev
```

### 15.3 Build para Produção

```bash
npm run build
npm run preview
```

---

## 16. Conclusão

O **CentroRecursos** é uma solução completa e moderna para gestão de recursos institucionais, combinando:

- **Tecnologias Atuais**: React, TypeScript, Supabase
- **Arquitetura Escalável**: BaaS com PostgreSQL
- **Segurança Robusta**: RLS e RBAC
- **UX Excepcional**: Interface intuitiva e responsiva
- **Funcionalidades Completas**: Do catálogo à manutenção

A aplicação resolve efetivamente os problemas de ineficiência e falta de transparência nos processos manuais de gestão de recursos, oferecendo uma plataforma centralizada, segura e fácil de usar para instituições educacionais e empresariais.
