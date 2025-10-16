# fintrack-angular

App de estudo para **teste técnico**: **Angular** + **json-server**, com **CRUD** de categorias e transações, **filtros** e **paginação**, rodando 100% no **GitHub Codespaces**.

> **Objetivo**: praticar front-end Angular consumindo uma API REST local, com foco em organização profissional (issues, PRs, CI, testes e padrões de commit).

---

## 🔎 Decisões iniciais

- Estrutura em **subpasta**: app Angular em `./frontend/`.
- **SSR/SSG**: **desativado** (MVP focado em CRUD/API).
- **Detecção de mudanças**: com **`zone.js`** (modo padrão, não “zoneless”).
- **Integrações de AI**: **nenhuma** (limpeza de escopo no MVP).
- **Analytics do Angular CLI**: **desativado** (telemetria off).

---

## 📌 Funcionalidades (MVP)

- **Categorias (income/expense)**: criar, listar, editar e excluir.
- **Transações**: tipo (renda/despesa), valor (> 0), data, categoria, descrição.
- **Lista com filtros**: por tipo, categoria e período.
- **Paginação** e **ordenação por data**.
- **Dashboard**: totais de renda, despesa e saldo (cálculo no front-end).

> **Glossário rápido**  
> **CRUD**: Create/Read/Update/Delete (operações básicas de dados).  
> **MVP**: Minimum Viable Product — versão mínima funcional.  
> **REST**: estilo de API baseada em recursos (GET/POST/PUT/PATCH/DELETE).

---

## 🧰 Stack

- **Angular** (CLI moderna, componentes _standalone_, Reactive Forms, HttpClient)
- **TypeScript**
- **json-server** (mock de API local)
- **GitHub Codespaces** (dev container)
- **Jasmine/Karma** (unit tests)
- **GitHub Actions** (CI — opcional)

---

## 🚀 Como rodar no GitHub Codespaces

> **Pré-requisito**: o app Angular foi gerado em `./frontend/` com `ng new frontend --routing --style=css --skip-git`.

### Opção A — processos separados (claro e explícito)

**Terminal 1** — API mock (na raiz do repositório):

```bash
npm run api
# ou, se ainda não criou script:
# npx json-server --watch db.json --port 3000
```

**Terminal 2** — Angular (na pasta do app):

```bash
cd frontend
npm start
# equivalente: ng serve --host 0.0.0.0 --port 4200
```

- A porta **4200** abrirá o Angular dev server.
- A porta **3000** expõe a API mock.

### Opção B — orquestrado pela raiz (quando você criar os scripts)

Na **raiz** do repo:

```bash
npm run dev
# Internamente executa: API (3000) + Angular (4200) em paralelo
```

---

## 💾 API Mock (json-server)

- Arquivo de dados: `db.json` na raiz do repo.
- Recursos:

  - `/categories` → `{ id, name, kind: "income" | "expense" }`
  - `/transactions` → `{ id, type, amount, date, categoryId, description? }`

**Filtros úteis**:

- `GET /transactions?type=expense`
- `GET /transactions?categoryId=3`
- `GET /transactions?date_gte=2025-10-01&date_lte=2025-10-31`
- `GET /transactions?_page=1&_limit=10&_sort=date&_order=desc`

> **Paginação**: leia o header `x-total-count` para saber o total de itens.

---

## 🏗️ Estrutura sugerida

```txt
fintrack-angular/
├─ .devcontainer/           # Definição do ambiente Codespaces (Dockerfile, devcontainer.json)
├─ .vscode/                 # Extensões recomendadas e settings de editor
├─ db.json                  # Base da API mock (json-server)
├─ fintrack/                # App Angular (gerado via Angular CLI)
│  ├─ src/
│  │  ├─ app/
│  │  │  ├─ core/          # interceptor, services base, guard, tokens
│  │  │  ├─ features/      # categories, transactions, dashboard
│  │  │  └─ shared/        # componentes/pipes/utilitários reaproveitáveis
│  │  └─ environments/     # environment.ts com apiUrl
│  └─ ...
└─ ...
```

---

## ⚙️ Configuração Angular (resumo)

- **Environment**: `frontend/src/environments/environment.ts`

  - `apiUrl = 'http://localhost:3000'`

- **HTTP Interceptor**: prefixa `apiUrl` em chamadas que não iniciem com `http`.
- **Rotas**:

  - `/categories`
  - `/transactions`
  - `/dashboard`

> **Standalone components**: componentes sem NgModule, mais simples e modernos.

---

## 🧪 Testes

- **Services**: use `HttpClientTestingModule` + `HttpTestingController`

  - Verifique método, URL, parâmetros e corpo da requisição.
  - Cubra cenários de erro (4xx/5xx).

- **Formulários**:

  - `amount > 0`
  - Validação cruzada: `category.kind === transaction.type`

> **Headless**: execução de testes sem abrir janela de browser — ideal para CI.

---

## 🤖 CI (opcional, recomendado)

Crie um workflow em `.github/workflows/ci.yml` para:

- `npm ci`
- `npm run build` (dentro de `./frontend/`)
- `npm test -- --watch=false --browsers=ChromeHeadless`

Ative **branch protection** na `main` exigindo **status checks** verdes antes do merge.

> **Status check**: verificação automática (build/testes) que bloqueia PR com falhas.

---

## 🗂️ Gestão por Issues e PRs

- Crie **issues** pequenas com **critérios de aceite** claros (Definition of Done).
- Fluxo recomendado:

  1. **issue** → branch `feat/<slug>` (ou `fix/`, `chore/`…)
  2. **commits pequenos** no padrão **Conventional Commits**
  3. **PR** referenciando a issue (template, prints e “como testar”)
  4. Review + CI verde → **merge**

**Labels sugeridos**: `feature`, `bug`, `chore`, `docs`, `test`
**Conventional Commits**: `feat:`, `fix:`, `chore:`, `docs:`, `test:` (use verbo no imperativo)

---

## 🛣️ Roadmap

- [ ] CRUD de **categorias**
- [ ] CRUD de **transações**
- [ ] **Filtros** por tipo/categoria/período
- [ ] **Paginação** e ordenação por data
- [ ] **Dashboard** (totais e saldo)
- [ ] **Testes** (services + forms)
- [ ] **CI** no GitHub Actions

---

## 📎 Licença

[MIT](LICENSE)
