# SIGEM — Guia de Instalação Completo

## Pré-requisitos

- Node.js 18+ → [nodejs.org](https://nodejs.org)
- Conta gratuita no Supabase → [supabase.com](https://supabase.com)

---

## Passo 1 — Criar base de dados no Supabase

1. Aceda a [supabase.com](https://supabase.com) e crie uma conta
2. Clique **"New Project"**
3. Preencha:
   - **Name**: `sigem-db`
   - **Database Password**: crie uma password forte e guarde-a
   - **Region**: West EU (ou a mais próxima)
4. Aguarde ~2 minutos

### Obter a URL de ligação

1. No painel Supabase → **Settings → Database**
2. Em **Connection string** → seleccione **URI**
3. Copie — será algo como:
   ```
   postgresql://postgres:[PASSWORD]@db.abcdefgh.supabase.co:5432/postgres
   ```
4. Substitua `[PASSWORD]` pela password criada

---

## Passo 2 — Configurar o projecto

```bash
# Instalar dependências
npm install

# Copiar ficheiro de ambiente
cp .env.example .env
```

Abra o `.env` e preencha:

```env
DATABASE_URL="postgresql://postgres:SUA_PASSWORD@db.SEU_REF.supabase.co:5432/postgres"
DIRECT_URL="postgresql://postgres:SUA_PASSWORD@db.SEU_REF.supabase.co:5432/postgres"
```

Gere um JWT_SECRET seguro:
```bash
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```
Cole o resultado no `.env`:
```env
JWT_SECRET="resultado_do_comando_acima"
```

---

## Passo 3 — Criar as tabelas

```bash
npm run db:generate
npm run db:migrate
# Quando perguntar o nome: escreva "init"
```

Resultado esperado:
```
✓ Your database is now in sync with your schema.
```

---

## Passo 4 — Criar dados iniciais

```bash
npm run db:seed
```

Resultado:
```
✓ 11 províncias criadas
✓ 2 escolas criadas
✓ ADMIN_MEC    admin@minedh.gov.mz       /  Admin@SIGEM2025
✓ COORDENADOR  coordenador.maputo@...    /  Coord@SIGEM2025
✓ DIRECTOR     director@ep1maputo.mz     /  Direct@SIGEM2025
✓ PROFESSOR    professor@ep1maputo.mz    /  Prof@SIGEM2025
✓ DIRECTOR     director@quelimane.mz     /  Direct@SIGEM2025
```

---

## Passo 5 — Iniciar o servidor

```bash
npm run dev
```

```
SIGEM API em execução → http://localhost:3000
Health check         → http://localhost:3000/health
```

---

## Testar a API

### Health check
```bash
curl http://localhost:3000/health
```

### Login
```bash
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@minedh.gov.mz","password":"Admin@SIGEM2025"}'
```

Guarde o `accessToken` da resposta.

### Ver perfil
```bash
curl http://localhost:3000/api/auth/perfil \
  -H "Authorization: Bearer SEU_ACCESS_TOKEN"
```

### Listar utilizadores (Admin)
```bash
curl http://localhost:3000/api/auth/utilizadores \
  -H "Authorization: Bearer TOKEN_DO_ADMIN"
```

### Activar utilizador
```bash
curl -X PATCH http://localhost:3000/api/auth/utilizadores/ID/activar \
  -H "Authorization: Bearer TOKEN_DO_ADMIN"
```

---

## Ver a base de dados visualmente

```bash
npm run db:studio
# Abre http://localhost:5555
```

Ou vá directamente ao **Supabase → Table Editor**.

---

## Problemas comuns

**"Cannot connect to database"**
→ Verifique a `DATABASE_URL` no `.env`. Certifique-se que a password está correcta.

**"JWT_SECRET is required"**
→ O ficheiro `.env` existe? Tem o `JWT_SECRET` preenchido?

**"P2002: Unique constraint"**
→ O email já existe. Use outro email ou limpe: `npx prisma migrate reset`

**Porta 3000 ocupada**
→ Mude `PORT=3001` no `.env`

---

## Comandos úteis

```bash
npm run dev          # Servidor em modo desenvolvimento (hot reload)
npm run build        # Compilar para produção
npm run start        # Executar build de produção
npm run db:studio    # Interface visual da base de dados
npm run db:migrate   # Aplicar migrações
npm run db:seed      # Criar dados iniciais
```

---

⚠️ **ANTES DE IR PARA PRODUÇÃO:**
- Altere todas as passwords do seed
- Gere um novo `JWT_SECRET`
- Mude `NODE_ENV=production`
- Desactive os logs de debug
