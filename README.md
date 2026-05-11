# Projeto Go (iniciante/intermediário)

## “TaskFlow” — API de Gerenciamento de Tarefas com CLI

Um projeto excelente para evoluir em Go é criar uma pequena plataforma de tarefas estilo Trello simplificado:

* API REST em Go
* Banco de dados SQLite/PostgreSQL
* CLI para interagir com a API
* Autenticação JWT
* Concorrência com goroutines
* Logs e middleware
* Configuração via `.env`

Você vai aprender:

* Estrutura de projeto real
* HTTP servers
* JSON
* Banco de dados
* Contexts
* Concurrency
* Interfaces
* Tratamento de erros
* Testes
* Clean architecture básica

---

# O problema que o projeto resolve

Hoje muita gente usa:

* anotações soltas
* planilhas
* blocos de notas

A ideia do TaskFlow:

* usuários criam contas
* criam tarefas
* definem prioridade
* marcam como concluídas
* filtram tarefas
* usam terminal para interagir rapidamente

---

# Tecnologias obrigatórias

## Linguagem

* Go 1.22+

## Banco

Escolha UM:

* SQLite (mais fácil)
* PostgreSQL (mais profissional)

## Bibliotecas

Sugestão boa e simples:

### HTTP Router

* `github.com/gin-gonic/gin`
  OU
* `net/http` puro (mais aprendizado)

### JWT

* `github.com/golang-jwt/jwt/v5`

### ORM

* `gorm.io/gorm`

### SQLite driver

* `gorm.io/driver/sqlite`

### Variáveis de ambiente

* `github.com/joho/godotenv`

---

# Estrutura obrigatória do projeto

```txt
taskflow/
│
├── cmd/
│   └── api/
│       └── main.go
│
├── internal/
│   ├── handlers/
│   ├── services/
│   ├── repository/
│   ├── models/
│   ├── middleware/
│   ├── database/
│   └── auth/
│
├── pkg/
│   └── utils/
│
├── tests/
│
├── .env
├── go.mod
└── README.md
```

---

# Funcionalidades obrigatórias

# 1. Cadastro de usuário

## Requisitos

* nome
* email
* senha

## O que aprender

### Hash de senha

Você DEVE usar:

```go
bcrypt.GenerateFromPassword()
```

E validar com:

```go
bcrypt.CompareHashAndPassword()
```

---

# 2. Login com JWT

## Requisitos

* gerar token JWT
* validar token em rotas privadas

## Você DEVE usar

### Criar token

```go
jwt.NewWithClaims()
```

### Middleware

* ler Authorization header
* validar Bearer token
* salvar usuário no contexto

---

# 3. CRUD de tarefas

## Campos da tarefa

```go
type Task struct {
    ID          uint
    Title       string
    Description string
    Done        bool
    Priority    string
    UserID      uint
}
```

## Endpoints obrigatórios

### Criar tarefa

```http
POST /tasks
```

### Listar tarefas

```http
GET /tasks
```

### Atualizar tarefa

```http
PUT /tasks/:id
```

### Deletar tarefa

```http
DELETE /tasks/:id
```

---

# 4. Filtros e busca

## Requisitos

* buscar por prioridade
* buscar concluídas
* buscar por texto

Exemplo:

```http
GET /tasks?done=true&priority=high
```

---

# 5. Concorrência (parte MUITO importante)

Você DEVE usar:

* goroutines
* channels
* sync.WaitGroup

## Desafio

Criar sistema de notificações assíncronas.

Quando uma tarefa for criada:

* uma goroutine envia “email fake”
* outra grava logs
* outra atualiza métricas

Exemplo:

```go
go sendEmail(task)
go saveLog(task)
```

Depois faça versão melhor:

```go
var wg sync.WaitGroup
```

E também:

```go
channel := make(chan string)
```

---

# 6. Middleware

Você DEVE criar:

* logger middleware
* auth middleware
* recovery middleware

## Aprender:

### Context

```go
context.Context
```

### Tempo de requisição

```go
time.Now()
time.Since()
```

---

# 7. Banco de dados

## Você DEVE implementar:

* migrations automáticas
* relacionamento User -> Tasks

### Exemplo:

```go
db.AutoMigrate(&User{}, &Task{})
```

---

# 8. Tratamento de erros

Você NÃO pode:

```go
panic(err)
```

Você DEVE:

```go
if err != nil {
    return err
}
```

E criar erros customizados:

```go
errors.New()
fmt.Errorf()
```

---

# 9. Logs

Você DEVE usar:

```go
log.Println()
```

E depois evoluir para:

* logs estruturados

Sugestão:

* `log/slog` (Go moderno)

---

# 10. Configuração via .env

## Variáveis

```env
PORT=8080
JWT_SECRET=secret
DB_NAME=taskflow.db
```

Você DEVE usar:

```go
godotenv.Load()
os.Getenv()
```

---

# 11. Testes

## Você DEVE fazer:

### Testes unitários

```go
func TestCreateTask(t *testing.T)
```

### Table tests

Muito importante em Go:

```go
tests := []struct{
    name string
    input string
    expected string
}{
}
```

### Rodar testes

```bash
go test ./...
```

---

# 12. CLI em Go

Crie um terminal para consumir sua API.

## Exemplos

```bash
taskflow add "Estudar Go"
taskflow list
taskflow done 3
```

## Você vai aprender:

* flags
* argumentos
* HTTP client

Você DEVE usar:

```go
http.NewRequest()
http.Client{}
```

---

# Funcionalidades EXTRAS (nível intermediário)

## Cache em memória

Usar:

```go
sync.Map
```

---

## Rate limiting

Exemplo:

* máximo 10 requests por minuto

Aprender:

* mutex
* maps concorrentes

---

## Worker Pool

Faça um sistema:

* múltiplos workers processando jobs

Você DEVE usar:

```go
jobs := make(chan Job)
```

---

## Graceful Shutdown

Você DEVE usar:

```go
http.Server
signal.Notify()
context.WithTimeout()
```

---

# Conceitos do Go que você DEVE praticar

# Básico

* structs
* interfaces
* ponteiros
* slices
* maps
* métodos
* pacotes

# Intermediário

* interfaces
* composition
* context
* concurrency
* channels
* mutex
* goroutines

# Muito importante

* error handling idiomático
* dependency injection simples
* organização de projeto

---

# Fluxo completo do projeto

## Fase 1

* subir API
* conectar banco
* criar rotas

## Fase 2

* autenticação
* middleware
* JWT

## Fase 3

* CRUD completo

## Fase 4

* concorrência

## Fase 5

* testes

## Fase 6

* CLI

---

# Como executar

## Criar projeto

```bash
go mod init taskflow
```

## Rodar

```bash
go run cmd/api/main.go
```

---

# O que você terá aprendido ao final

Você vai sair sabendo:

* criar APIs reais
* usar banco
* autenticação
* concorrência em Go
* arquitetura
* testes
* organização profissional
* CLI
* middleware
* context

Isso já é suficiente para:

* estágio
* vaga júnior
* freelas pequenos
* projetos pessoais sérios

---

# Próximo nível depois desse

Quando terminar:

1. Dockerizar
2. Adicionar Redis
3. WebSocket
4. CI/CD
5. Kubernetes básico
6. Deploy na VPS
7. Observabilidade (Prometheus/Grafana)

---

# Desafio bônus MUITO bom

Adicionar:

* sistema de equipes
* tarefas compartilhadas
* websocket para updates em tempo real

Aí o projeto sobe MUITO de nível.
