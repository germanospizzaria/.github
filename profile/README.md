# 🚚 EntregaAi - Docker Deployment

## 🚀 Início Rápido

### 1. Configuração Inicial

```bash
# Repositório da aplicação
git clone <url-do-repo-entregaai>
cd entregaai

# Repositório de deploy (em outro diretório)
git clone <url-do-repo-entregaai-deploy>
cd entregaai-deploy
```

### 2. Edite o arquivo `.env`

⚠️ **IMPORTANTE**: configure suas variáveis corretamente.

```bash
POSTGRES_PASSWORD=sua_senha_segura_aqui
JWT_SECRET=sua_chave_jwt_super_secreta_aqui
```

### 3. Inicie a aplicação

```bash
./deploy.sh start
# ou
docker-compose up --build -d
```

### 4. Execute as migrações do banco

```bash
./deploy.sh migrate
./deploy.sh seed   # opcional
```

---

## 🌐 Acessos

* **Frontend** → [http://localhost:8080](http://localhost:8080)
* **API** → [http://localhost:3000](http://localhost:3000)
* **Database** → localhost:5432

---

## 📜 Comandos Disponíveis

### Script de Deploy

```bash
./deploy.sh start              # Inicia a aplicação
./deploy.sh start production   # Inicia com proxy nginx
./deploy.sh stop               # Para a aplicação
./deploy.sh restart            # Reinicia a aplicação
./deploy.sh logs               # Mostra logs
./deploy.sh migrate            # Executa migrações
./deploy.sh seed               # Popula o banco
./deploy.sh cleanup            # Remove tudo
```

### Docker Compose Direto

```bash
docker-compose up -d --build
docker-compose down
docker-compose logs -f
docker-compose exec api npx prisma migrate deploy
```

---

## 🏗️ Arquitetura dos Containers

* **Backend (entregaai-api)**
  Node.js 20 (Alpine) | \~150MB | Porta 3000

* **Frontend (entregaai-front)**
  Nginx (Alpine) | \~25MB | Porta 8080

* **Database (PostgreSQL 17)**
  Alpine | \~80MB | Porta 5432
  
---

## ⚡ Otimizações

* **Tamanho**: multi-stage builds, Alpine Linux
* **Segurança**: non-root users, headers de segurança, rede isolada
* **Performance**: health checks, gzip, cache otimizado, pooling
* **Recursos**: memory e CPU limits, restart policies

---

## 🔧 Customização

### Variáveis de Ambiente

```bash
API_PORT=3001
FRONTEND_PORT=8081
POSTGRES_DB=meu_banco
NODE_ENV=production
JWT_SECRET=sua_chave
```

### Backup/Restore Banco

```bash
docker-compose exec database pg_dump -U postgres entregaai > backup.sql
docker-compose exec -T database psql -U postgres entregaai < backup.sql
```

---

## 🐛 Troubleshooting

```bash
docker-compose ps              # status
docker-compose logs -f api     # logs detalhados
docker-compose restart api     # restart de serviço
docker-compose build --no-cache
docker-compose up -d --force-recreate
docker image prune             # limpar imagens não usadas
```

---

## 🔐 Produção

1. Use HTTPS no nginx
2. Senhas fortes e únicas
3. Backup regular do banco
4. Monitoramento e logs centralizados
5. Atualize imagens frequentemente

```bash
./deploy.sh start production
```

---

## 📊 Monitoramento

### Health Checks

```bash
curl http://localhost:3000/health   # API
curl http://localhost:8080/health   # Frontend
```

### Logs

```bash
docker-compose logs -f
docker-compose logs -f api
```

---

🏗️ Estrutura dos Repositórios

entregaai → código da aplicação (frontend e backend)

entregaai-deploy → arquivos de infraestrutura para deploy com Docker

---

## 🤝 Contribuição

1. Fork do projeto
2. Branch para sua feature
3. Teste localmente com Docker
4. Pull Request

---
