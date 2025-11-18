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

## Possíveis Evoluções

- Persistência com `Flask-SQLAlchemy` (já no `requirements.txt`).
- Validação de entrada com `marshmallow`/`pydantic` ou validações manuais.
- Paginação, filtros e ordenação no `GET /tasks`.
- Habilitar CORS com `Flask-Cors` para consumo por um frontend.
- Documentação interativa (Swagger/OpenAPI) com `flask-restx` ou `apispec`.
- Testes automatizados com `pytest` e `pytest-flask`.
- Camadas de serviço/repositório para separar regras de negócio.

## O que este projeto demonstra

- Construção de API REST com Python + Flask.
- Modelagem simples, serialização e contratos JSON claros.
- Organização mínima de código e documentação voltada a recrutadores.

---

Se quiser, posso evoluir este projeto para persistência real, documentação OpenAPI e testes — basta pedir! :)
