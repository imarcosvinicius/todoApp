# TodoApp

Estrutura inicial do projeto de API de Tarefas (Todo App) desenvolvida em **Python 3.13** com **FastAPI** e gerenciada via **[uv](https://docs.astral.sh/uv/)**.

---

## 🛠️ Stack Atual

- **Linguagem:** Python >= 3.13
- **Gerenciador de Pacotes e Ambiente:** [uv](https://github.com/astral-sh/uv)
- **Framework Web:** FastAPI (com `fastapi[standard]` incluindo Uvicorn e FastAPI CLI)
- **ORM / Banco de Dados:** SQLAlchemy >= 2.1.6

---

## 🚀 Como Executar

### Pré-requisitos
- [uv](https://docs.astral.sh/uv/getting-started/installation/) instalado.
- Python 3.13 (o `uv` gerencia e baixa a versão automaticamente se necessário).

### 1. Clonar o repositório
```bash
git clone <URL_DO_REPOSITORIO>
cd todoApp
```

### 2. Sincronizar o ambiente virtual e dependências
```bash
uv sync
```

### 3. Executar o script inicial
```bash
uv run todoapp
```

---

## 🏛️ Arquitetura Recomendada para o Projeto

Para a evolução do projeto seguindo boas práticas de separação de responsabilidades, arquitetura em camadas e modularidade no ecossistema FastAPI, recomenda-se a seguinte estrutura de diretórios em `src/todoapp/`:

```text
src/todoapp/
├── api/
│   ├── v1/
│   │   ├── endpoints/
│   │   │   ├── auth.py          # Autenticação e tokens JWT
│   │   │   ├── todos.py         # Endpoints CRUD de tarefas (/todos)
│   │   │   └── users.py         # Endpoints de cadastro e gestão de usuários
│   │   └── api.py               # Agrupador de rotas da versão 1
│   └── deps.py                  # Injeção de dependências (get_db, get_current_user)
├── core/
│   ├── config.py                # Configurações da aplicação e variáveis de ambiente
│   └── security.py              # Hashing de senhas e geração/validação de tokens JWT
├── db/
│   ├── base.py                  # Classe DeclarativeBase do SQLAlchemy 2.0
│   └── session.py               # Engine e SessionLocal (gerenciador de conexão get_db)
├── models/
│   ├── todo.py                  # Modelo ORM Todo (SQLAlchemy 2.0 com Mapped[])
│   └── user.py                  # Modelo ORM User
├── schemas/
│   ├── todo.py                  # Schemas Pydantic (TodoCreate, TodoUpdate, TodoResponse)
│   └── user.py                  # Schemas Pydantic (UserCreate, UserResponse, Token)
└── main.py                      # Instância principal do FastAPI, middlewares e inclusão de routers
```

### Responsabilidade das Camadas:
- **`api/`**: Responsável pela camada de transporte HTTP, definição de rotas, status codes e injeção de dependências (`Depends`).
- **`core/`**: Configurações centrais, parâmetros de segurança, hashing e leitura de variáveis de ambiente.
- **`db/`**: Configuração da engine de banco de dados e ciclo de vida de sessões (`yield` / `db.close()`).
- **`models/`**: Definição das tabelas e relacionamentos ORM no banco de dados.
- **`schemas/`**: DTOs (Data Transfer Objects) e contratos de dados validados via Pydantic V2 para entrada e saída de dados.
- **`main.py`**: Ponto de entrada do servidor ASGI, montagem dos roteadores e eventos de ciclo de vida (Lifespan).
