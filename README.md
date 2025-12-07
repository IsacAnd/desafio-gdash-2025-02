# 🌦️ Weather Insight System — Full Stack com IA, Mensageria e Microsserviços

* **LINK DO VÍDEO DE APRESENTAÇÃO: https://youtu.be/3E3XmT_mhB4** 

Sistema completo para coleta, processamento, armazenamento, visualização e geração de **insights inteligentes climáticos**, utilizando:

* **Frontend:** React + Vite
* **Backend:** NestJS
* **Banco de Dados:** MongoDB
* **Mensageria:** RabbitMQ
* **Workers:** Go + Python
* **IA:** DeepSeek via OpenRouter
* **Infraestrutura:** Docker & Docker Compose

---

## 🧠 Visão Geral da Arquitetura

```
[ Producer (Python) ]
          |
          v
     [ RabbitMQ ]
          |
          v
    [ Worker (Go) ] ---> [ Backend (NestJS) ] ---> [ MongoDB ]
                                   |
                                   v
                           [ DeepSeek (IA) ]
                                   |
                                   v
                           [ Frontend (React) ]
```

---

## 🚀 Tecnologias Utilizadas

### Backend

* NestJS
* Mongoose
* JWT
* Bcrypt
* ConfigModule
* OpenRouter (DeepSeek)

### Frontend

* React
* Vite
* TailwindCSS
* React Router

### Infra

* Docker
* Docker Compose
* MongoDB
* RabbitMQ

### Workers

* Python Producer (coleta de dados)
* Go Worker (processamento)
* IA para geração de insights

---

## ✅ Pré-requisitos

Antes de começar, você precisa ter instalado:

* ✅ Docker
* ✅ Docker Compose
* ✅ Node.js 20+ (somente se for rodar localmente)
* ✅ Git

---

## 📁 Estrutura do Projeto

```
/
├── backend/
├── frontend/
├── producer/
├── worker/
├── docker-compose.yml
├── .env
└── README.md
```

---

## 🔐 Variáveis de Ambiente (`.env`)

Crie um arquivo `.env` na raiz do projeto:

```env
# =========================
# RABBITMQ
# =========================
RABBITMQ_USER=guest
RABBITMQ_PASS=guest
RABBITMQ_HOST=rabbitmq
RABBITMQ_PORT=5672
RABBITMQ_MANAGEMENT_PORT=15672
RABBITMQ_QUEUE=weather_queue
RABBITMQ_URL=amqp://guest:guest@rabbitmq:5672/

# =========================
# MONGODB
# =========================
MONGO_INITDB_ROOT_USERNAME=root
MONGO_INITDB_ROOT_PASSWORD=example
MONGO_HOST=mongo
MONGO_PORT=27017
MONGO_DB=weather_db

# =========================
# BACKEND
# =========================
BACKEND_PORT=3000
WORKER_SECRET=change_me_to_a_strong_secret
JWT_SECRET=change_this_jwt_secret
BACKEND_INTERNAL_URL=http://backend:3000/api/weather/logs

# =========================
# FRONTEND
# =========================
FRONTEND_PORT=8080
VITE_BACKEND_URL=http://localhost:3000
VITE_API_BASE_URL=http://localhost:3000/api

# =========================
# PRODUCER
# =========================
LAT=-3.71722
LON=-38.5434
INTERVAL_SECONDS=10
OPEN_METEO_URL=https://api.open-meteo.com/v1/forecast

# =========================
# GO WORKER
# =========================
NEST_BASE_URL=http://backend:3000

# =========================
# DEEPSEEK (IA)
# =========================
DS_API_KEY=sk-or-v1-xxxxxxxxxxxxxxxxxxxxxxxx
DS_MODEL=tngtech/deepseek-r1t2-chimera:free
DS_API_URL=https://openrouter.ai/api/v1/chat/completions
```

---

---

## ⚠️ IMPORTANTE — Criar o arquivo `.env`

Este projeto **não versiona o arquivo `.env` por segurança**.  
Após clonar o repositório, você **DEVE criar o seu próprio `.env` local** antes de subir os containers.

### ✅ Passo a passo:

No **Windows (PowerShell)**:

```powershell
copy .env.example .env
```

---

## 🤖 Criar Conta no OpenRouter (OBRIGATÓRIO para a IA funcionar)

Para usar a IA DeepSeek gratuitamente:

1. Acesse:
   👉 [https://openrouter.ai](https://openrouter.ai)

2. Crie sua conta (GitHub ou Google).

3. No painel, acesse **API Keys**.

4. Gere sua chave de API.

5. Copie a chave e cole no `.env`:

```env
DS_API_KEY=sk-or-v1-SUA_CHAVE_AQUI
```

✅ Sem essa chave, o sistema **não consegue gerar os insights automáticos**.

---

## 🐳 Como Rodar o Sistema com Docker (RECOMENDADO)

### ✅ 1. Clonar o projeto

```bash
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio
```

---

### ✅ 2. Subir todos os serviços

```bash
docker compose up --build -d
```

---

### ✅ 3. Serviços e URLs

| Serviço     | URL                                              |
| ----------- | ------------------------------------------------ |
| Frontend    | [http://localhost:8080](http://localhost:8080)   |
| Backend API | [http://localhost:3000](http://localhost:3000)   |
| RabbitMQ    | [http://localhost:15672](http://localhost:15672) |
| MongoDB     | mongodb://localhost:27017                        |

---

## 🔑 Autenticação

* Sistema possui **Login e Registro**
* Autenticação feita via **JWT**
* Rotas protegidas no frontend com:

  * `PrivateRoute` (para usuários logados)
  * `PublicRoute` (bloqueia login/registro se já estiver logado)

---

## 📊 Fluxo de Funcionamento

1. Producer (Python) coleta dados climáticos da Open-Meteo.
2. Envia os dados para o RabbitMQ.
3. Worker (Go) consome a fila.
4. Worker envia os dados para o Backend.
5. Backend salva os dados no MongoDB.
6. Backend envia os dados para a IA (DeepSeek).
7. IA gera insights inteligentes.
8. Frontend exibe os dados e os insights em tempo real.

---

## 🛑 Problemas Comuns

### ❌ Erro de autenticação no MongoDB

Execute:

```bash
docker compose down -v
docker compose up --build -d
```

---

### ❌ Erro `data and salt arguments required` no bcrypt

Certifique-se de que existem no `.env`:

```env
DEFAULT_ADMIN_EMAIL=admin@example.com
DEFAULT_ADMIN_PASSWORD=123456
```

---

### ❌ IA não gera insights

Verifique se existe no `.env`:

```env
DS_API_KEY=sk-or-v1-...
```

---

## 🧪 Rodar o Sistema Sem Docker (Opcional)

### Backend

```bash
cd backend
npm install
npm run start:dev
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

---

## ✅ Status do Projeto

* ✅ Backend completo
* ✅ Autenticação com JWT
* ✅ Workers funcionando
* ✅ Mensageria RabbitMQ
* ✅ Integração com IA (DeepSeek)
* ✅ Dashboard em tempo real
* ✅ Sistema totalmente dockerizado

---

## 👨‍💻 Autor

Projeto desenvolvido por **Isac Andrade**
Área: Full Stack, IA, Microsserviços, Mensageria e Cloud
