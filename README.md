# SIGEM — Sistema de Gestão Escolar de Moçambique

> Plataforma nacional de administração educacional para o Ministério da Educação e Desenvolvimento Humano (MINEDH).

![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)
![Node](https://img.shields.io/badge/node-18%2B-green)
![TypeScript](https://img.shields.io/badge/TypeScript-5.4-blue)
![Licença](https://img.shields.io/badge/licença-proprietária-red)

---

## Sobre o Projecto

O SIGEM é um sistema integrado de gestão escolar desenvolvido especificamente para o contexto educacional moçambicano. Permite ao MINEDH, directores de escola e coordenadores regionais gerir matrículas, acompanhar o desempenho académico e gerar relatórios estatísticos em tempo real.

---

## Estrutura do Projecto

```
sigem/
├── prisma/
│   ├── schema.prisma        # Esquema da base de dados
│   └── seed.ts              # Dados iniciais (províncias, escolas, utilizadores)
├── src/
│   ├── config/
│   │   ├── index.ts         # Variáveis de ambiente
│   │   └── prisma.ts        # Cliente da base de dados
│   ├── controllers/
│   │   ├── auth.service.ts  # Lógica de negócio da autenticação
│   │   └── auth.controller.ts # Handlers HTTP
│   ├── middleware/
│   │   ├── auth.ts          # Verificação JWT e papéis
│   │   └── errorHandler.ts  # Tratamento global de erros
│   ├── routes/
│   │   └── auth.routes.ts   # Endpoints de autenticação
│   ├── types/
│   │   ├── enums.ts         # Enumerações (Role, Status...)
│   │   └── index.ts         # Tipos TypeScript
│   ├── utils/
│   │   ├── jwt.ts           # Geração e verificação de tokens
│   │   ├── logger.ts        # Sistema de logs
│   │   └── response.ts      # Helpers de resposta HTTP
│   ├── app.ts               # Configuração Express
│   └── server.ts            # Ponto de entrada
├── docs/
│   ├── index.html           # Landing page do produto
│   └── demo.html            # Protótipo do dashboard
├── .env.example             # Variáveis necessárias (sem passwords)
├── .gitignore
├── package.json
├── tsconfig.json
└── SETUP.md                 # Guia de instalação passo a passo
```

---

## Módulos

| Módulo | Estado |
|--------|--------|
| Autenticação (JWT + Papéis) | ✅ Concluído |
| API de Escolas | 🔄 Em desenvolvimento |
| API de Alunos | 🔄 Em desenvolvimento |
| API de Professores | 📅 Planeado |
| Notas e Avaliações | 📅 Planeado |
| Relatórios e Estatísticas | 📅 Planeado |
| Frontend React (Dashboard) | 📅 Planeado |

---

## Papéis de Utilizador

| Papel | Acesso |
|-------|--------|
| `ADMIN_MEC` | Acesso total ao sistema nacional |
| `COORDENADOR` | Dados da sua região/província |
| `DIRECTOR` | Dados da sua escola |
| `PROFESSOR` | Apenas as suas turmas e disciplinas |

---

## Stack Tecnológico

- **Runtime**: Node.js 18+ com TypeScript
- **Framework**: Express.js
- **Base de Dados**: PostgreSQL via Supabase
- **ORM**: Prisma
- **Autenticação**: JWT (access token 24h + refresh token 7 dias)
- **Segurança**: Helmet, CORS, Rate Limiting, bcrypt
- **Logs**: Winston
- **Validação**: Zod

---

## Instalação Rápida

```bash
# 1. Instalar dependências
npm install

# 2. Configurar ambiente
cp .env.example .env
# Editar .env com as credenciais do Supabase e JWT_SECRET

# 3. Criar tabelas na base de dados
npm run db:generate
npm run db:migrate

# 4. Criar dados iniciais
npm run db:seed

# 5. Iniciar servidor de desenvolvimento
npm run dev
```

Consulte o **[SETUP.md](./SETUP.md)** para o guia completo com instruções do Supabase.

---

## Endpoints da API

### Autenticação (público)
| Método | Rota | Descrição |
|--------|------|-----------|
| `POST` | `/api/auth/login` | Login |
| `POST` | `/api/auth/registar` | Criar conta |
| `POST` | `/api/auth/refresh` | Renovar tokens |
| `POST` | `/api/auth/logout` | Terminar sessão |

### Perfil (autenticado)
| Método | Rota | Descrição |
|--------|------|-----------|
| `GET` | `/api/auth/perfil` | Ver perfil próprio |
| `PUT` | `/api/auth/password` | Alterar password |

### Administração (apenas ADMIN_MEC)
| Método | Rota | Descrição |
|--------|------|-----------|
| `GET` | `/api/auth/utilizadores` | Listar todos |
| `PATCH` | `/api/auth/utilizadores/:id/activar` | Activar conta |
| `PATCH` | `/api/auth/utilizadores/:id/suspender` | Suspender conta |

---

## Segurança

- Passwords com bcrypt (12 rounds)
- JWT com rotação de refresh tokens
- Rate limiting: 10 tentativas de login por 15 minutos
- Headers de segurança via Helmet
- CORS configurado por origens
- Auditoria de todas as acções sensíveis
- Tempo constante no login (sem enumeração de emails)

---

## Requisitos Não-Funcionais

| Requisito | Meta |
|-----------|------|
| Performance | < 2s por consulta |
| Disponibilidade | 99,5% |
| Escalabilidade | 1.000+ escolas |
| Offline-first | Previsto na Fase 2 |

---

## Roadmap

### Fase 1 — Autenticação e Base (actual)
- [x] Schema da base de dados
- [x] Sistema de autenticação JWT com papéis
- [x] Auditoria de operações
- [ ] API de Escolas
- [ ] API de Alunos

### Fase 2 — Core
- [ ] Notas e avaliações
- [ ] Frequência escolar
- [ ] Relatórios e estatísticas
- [ ] Exportação PDF/Excel

### Fase 3 — Escala Nacional
- [ ] Frontend React (Dashboard)
- [ ] Suporte offline-first
- [ ] Integração com sistemas do MINEDH
- [ ] Suporte a línguas locais

---

*© 2025 SIGEM. Desenvolvido para o Ministério da Educação e Desenvolvimento Humano de Moçambique.*
