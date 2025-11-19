# API de Tarefas (Flask)

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Flask](https://img.shields.io/badge/Flask-2.3.x-green)
![Status](https://img.shields.io/badge/status-educacional-orange)

Projeto simples e didático de uma API REST para gerenciamento de tarefas (To‑Do) construída com Flask. O objetivo é demonstrar, de forma objetiva, competências de construção de APIs, modelagem simples, boas práticas de organização e clareza na documentação.

## Visão Geral

- CRUD completo de tarefas em memória (sem banco de dados).
- Modelo `Task` com serialização via `to_dict()`.
- Endpoints REST em `app.py` com respostas JSON.
- Mensagens e exemplos em português para facilitar a leitura.

> Observação: este projeto é intencionalmente minimalista para fins de aprendizado. O arquivo `requirements.txt` inclui dependências para futuras evoluções (por exemplo, `Flask-SQLAlchemy`), mas o app atual não persiste dados.

## Tecnologias

- Python 3.10+
- Flask 2.3.x
- (Opcional para evoluções) Flask-CORS, Flask-SQLAlchemy

## Estrutura do Projeto

```
app.py
models/
  └── task.py
requirements.txt
```

- `app.py`: define a aplicação Flask e os endpoints.
- `models/task.py`: define a classe `Task` (id, title, description, completed).
- `requirements.txt`: dependências do projeto.

## Como Rodar Localmente

1. Crie e ative um ambiente virtual (Windows + Git Bash):

```bash
python -m venv .venv
source .venv/Scripts/activate
```

2. Instale as dependências:

```bash
pip install -r requirements.txt
```

3. Execute a aplicação:

```bash
python app.py
```

- Servidor sobe em `http://127.0.0.1:5000` (modo debug habilitado).

> Dica: para desativar o ambiente virtual, use `deactivate`.

## Endpoints

Base URL: `http://127.0.0.1:5000`

### Criar tarefa

- Método: `POST`
- Rota: `/tasks`
- Body (JSON):

```json
{
  "title": "Estudar Flask",
  "description": "Ler a documentação e praticar"
}
```

- Respostas:
  - `201 Created`
  ```json
  { "message": "Nova tarefa criada com sucesso!" }
  ```

### Listar tarefas

- Método: `GET`
- Rota: `/tasks`
- Resposta `200 OK`:

```json
{
  "tasks": [
    {
      "id": 1,
      "title": "Estudar Flask",
      "description": "Ler a documentação e praticar",
      "completed": false
    }
  ],
  "total_tasks": 1
}
```

### Buscar tarefa por ID

- Método: `GET`
- Rota: `/tasks/<id>`
- Exemplo: `/tasks/1`
- Respostas:
  - `200 OK` com o objeto da tarefa;
  - `404 Not Found`:
  ```json
  { "message": "Não foi possivel encontrar a atividade" }
  ```

### Atualizar tarefa

- Método: `PUT`
- Rota: `/tasks/<id>`
- Body (JSON) esperado:

```json
{
  "title": "Estudar Flask (revisado)",
  "description": "Refazer exemplos",
  "completed": true
}
```

- Respostas:
  - `200 OK`:
  ```json
  { "message": "Tarefa atualizada com sucesso!" }
  ```
  - `404 Not Found` se o ID não existir.

> Observação: o endpoint `PUT` espera as três chaves (`title`, `description`, `completed`). O envio parcial pode causar erro de validação (não implementada nesta versão minimalista).

### Deletar tarefa

- Método: `DELETE`
- Rota: `/tasks/<id>`
- Respostas:
  - `200 OK`:
  ```json
  { "message": "Tarefa deletada com sucesso!" }
  ```
  - `404 Not Found` se o ID não existir.

## Exemplos (curl)

Criar tarefa:

```bash
curl -X POST http://127.0.0.1:5000/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"Estudar Flask","description":"Ler a documentação"}'
```

Listar tarefas:

```bash
curl http://127.0.0.1:5000/tasks
```

Buscar por ID:

```bash
curl http://127.0.0.1:5000/tasks/1
```

Atualizar tarefa:

```bash
curl -X PUT http://127.0.0.1:5000/tasks/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"Estudar Flask (revisado)","description":"Refazer exemplos","completed":true}'
```

Deletar tarefa:

```bash
curl -X DELETE http://127.0.0.1:5000/tasks/1
```

## Decisões e Boas Práticas

- Mantido simples e legível: foco em clareza do fluxo CRUD.
- Serialização explícita no modelo (`to_dict`) para respostas consistentes.
- Mensagens de erro e sucesso padronizadas.
- Endpoints separados por responsabilidade e verbos HTTP adequados.

## Testes Automatizados

Esta API inclui um conjunto inicial de testes de integração em `tests.py` utilizando `pytest` e o pacote `requests` para fazer chamadas HTTP reais contra a instância local do servidor Flask.

### O que é coberto

- Criação de tarefa (`POST /tasks`)
- Listagem geral (`GET /tasks`)
- Busca por ID (`GET /tasks/<id>`)
- Atualização (`PUT /tasks/<id>`)
- Exclusão (`DELETE /tasks/<id>`)

> Estes testes exercitam o fluxo CRUD completo. Eles funcionam como testes de integração (não unitários) porque dependem da aplicação rodando e do estado em memória.

### Pré‑requisitos

1. Certifique-se de ter instalado as dependências (já inclui `pytest` e `requests`).
2. Inicie o servidor Flask em um terminal separado:

```bash
python app.py
```

3. Em outro terminal (ambiente virtual ativo), execute os testes:

```bash
python -m pytest
```

### Configuração do Pytest

O arquivo `pytest.ini` contém:

```ini
[pytest]
testpaths = .
python_files = tests.py
addopts = -v
```

Isso força o modo verboso e localiza o arquivo de teste único `tests.py` na raiz do projeto.

### Comandos úteis

Execução padrão (verboso já habilitado pelo `pytest.ini`):

```bash
python -m pytest
```

Execução silenciosa/minimalista:

```bash
python -m pytest -q
```

Parar no primeiro erro:

```bash
python -m pytest --maxfail=1
```

### Exemplo de saída (resumida)

```text
==================== test session starts ====================
platform win32 -- Python 3.10.x, pytest 7.4.x
collected 5 items

tests.py::test_create_task PASSED
tests.py::test_get_tasks PASSED
tests.py::test_get_task PASSED
tests.py::test_update_task PASSED
tests.py::test_delete_task PASSED

===================== 5 passed in 0.XXs =====================
```

### Melhorias futuras nos testes

- Adicionar asserts faltantes (algumas linhas usam `response.status_code == 200` sem `assert`).
- Usar `pytest-flask` ou fixtures para subir e derrubar o app automaticamente sem precisar de terminal separado.
- Isolar testes unitários para o modelo `Task` (ex.: comportamento de `to_dict`).
- Adicionar cobertura de código com `pytest-cov`:
  ```bash
  pip install pytest-cov
  python -m pytest --cov=. --cov-report=term-missing
  ```
- Integrar CI (GitHub Actions) para gerar badge de status de testes.

### Badge de Testes (opcional)

Após configurar CI, você pode adicionar algo como:

```markdown
![Tests](https://img.shields.io/badge/tests-passing-brightgreen)
```

## Possíveis Evoluções

- Persistência com `Flask-SQLAlchemy` (já no `requirements.txt`).
- Validação de entrada com `marshmallow`/`pydantic` ou validações manuais.
- Paginação, filtros e ordenação no `GET /tasks`.
- Habilitar CORS com `Flask-Cors` para consumo por um frontend.
- Documentação interativa (Swagger/OpenAPI) com `flask-restx` ou `apispec`.
- Camadas de serviço/repositório para separar regras de negócio.

## O que este projeto demonstra

- Construção de API REST com Python + Flask.
- Modelagem simples, serialização e contratos JSON claros.
- Organização mínima de código e documentação voltada a recrutadores.

---

Se quiser, posso evoluir este projeto para persistência real, documentação OpenAPI e testes — basta pedir! :)
