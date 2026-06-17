# Exemplos Práticos — MCP GitHub via Copilot

Código e queries prontos para usar no seu ambiente.

---

## 🔍 Query Examples

### Exemplo 1: Listar issues abertas recentes

**Pergunta ao Copilot:**
```
Liste as 10 issues abertas mais recentes do repositório microsoft/vscode
```

**MCP Chama:**
```
GET /repos/microsoft/vscode/issues?state=open&sort=created&direction=desc&per_page=10
```

**Resultado esperado:**
- Array JSON com 10 issues
- Copilot formata em tabela markdown
- Cada linha: `#número | título | autor | data`

---

### Exemplo 2: Buscar issues por palavra-chave

**Pergunta ao Copilot:**
```
Quais issues abertas no microsoft/vscode mencionam "memory leak" no título?
```

**MCP Chama:**
```
GET /search/issues?q=repo:microsoft/vscode is:open "memory leak" in:title
```

**Resultado esperado:**
- Todos os matches ordenados por relevância
- Conta total: `X issues encontradas`
- Top 5 exibidos

---

### Exemplo 3: Listar PRs mergeadas recentemente

**Pergunta ao Copilot:**
```
Mostre os últimos 5 pull requests mergeados no typescript-eslint/typescript-eslint
```

**MCP Chama:**
```
GET /repos/typescript-eslint/typescript-eslint/pulls?state=closed&sort=updated&direction=desc&per_page=5
```

**Resultado esperado:**
- PR título, autor, data de merge
- Branch info (branch de origem)
- SHA do commit

---

### Exemplo 4: Detalhes de uma issue com comentários

**Pergunta ao Copilot:**
```
Me dê os detalhes completos da issue #1234 do OWNER/REPO, incluindo todos os comentários
```

**MCP Chama (2 requisições):**
```
GET /repos/OWNER/REPO/issues/1234
GET /repos/OWNER/REPO/issues/1234/comments
```

**Resultado esperado:**
```json
{
  "number": 1234,
  "title": "...",
  "state": "open",
  "body": "...",
  "comments": [
    {
      "author": "...",
      "created_at": "...",
      "body": "..."
    }
  ]
}
```

---

### Exemplo 5: Buscar PRs por author

**Pergunta ao Copilot:**
```
Quais PRs abertas foram criadas por torvalds no repositório linux/linux?
```

**MCP Chama:**
```
GET /search/issues?q=repo:linux/linux is:pr is:open author:torvalds
```

---

## ✍️ Criando conteúdo (WRITE)

### Criar comentário em issue

**Pergunta ao Copilot:**
```
No repositório meu-usuario/meu-repo, adicione um comentário na issue #5 dizendo:
"Ótima issue! Estou trabalhando nisso."
```

**MCP Chama:**
```
POST /repos/meu-usuario/meu-repo/issues/5/comments
Content-Type: application/json

{
  "body": "Ótima issue! Estou trabalhando nisso."
}
```

**Fluxo seguro:**
1. Copilot mostra exatamente o texto que será postado
2. Você aprova com `OK` ou cancela
3. MCP executa POST
4. Comentário aparece em tempo real

---

### Criar um Pull Request

**Pergunta ao Copilot:**
```
Crie um pull request no meu-usuario/meu-repo:
- Título: "Adicionar documentação de setup"
- Descrição: "Guia passo-a-passo para configurar dev environment"
- Branch: docs/setup-guide → main
```

**MCP Chama:**
```
POST /repos/meu-usuario/meu-repo/pulls

{
  "title": "Adicionar documentação de setup",
  "body": "Guia passo-a-passo para configurar dev environment",
  "head": "docs/setup-guide",
  "base": "main"
}
```

---

## 🔐 Segurança

### ✅ Fazer
```powershell
# Usar variável de ambiente
$env:GITHUB_PERSONAL_ACCESS_TOKEN = "ghp_..."

# MCP vai ler desse ambiente
```

### ❌ NÃO fazer
```
# Nunca hardcode o token
const token = "ghp_..."; // ❌ RISCO

# Nunca commit em arquivo
git add .env  # ❌ Usa .gitignore!
```

### Escopos necessários do token

```
✅ repo          — acesso a repos (public + private)
✅ read:org      — leitura de organizações
✅ workflow      — (opcional) gerenciar Actions
```

---

## 📊 Padrões comuns

### Pattern 1: Buscar + Filtrar

```
"Liste todas as issues abertas do microsoft/vscode que não têm milestone"
```

Internamente:
1. Lista issues abertas
2. MCP filtra apenas as sem milestone
3. Retorna resultado consolidado

### Pattern 2: Compor múltiplas chamadas

```
"Mostre os últimos 3 PRs mergeados e para cada um, liste os commits inclusos"
```

Internamente:
1. Busca 3 PRs mergeadas ← 1 call
2. Para cada PR, busca commits ← 3 calls
3. Consolida em resposta unificada

### Pattern 3: Busca com múltiplos filtros

```
"Issues abertas do react/react criadas nos últimos 7 dias por membros da organização"
```

MCP Traduz para:
```
repo:facebook/react 
is:issue 
is:open 
created:>2026-06-10 
author:orgs/facebook
```

---

## 🎯 Casos de Uso Reais

### Use Case 1: Triagem de Issues

```
"No React, quantas issues abertas mencionam 'refactor' 
 e estão há mais de 30 dias sem atividade?"
```

**Valor:** Encontrar work stale sem sair do VS Code

### Use Case 2: Code Review via MCP

```
"Mostre-me os últimos 5 PRs do meu-usuario/meu-projeto 
 e para cada um, liste os arquivos modificados"
```

**Valor:** Review completo sem abrir GitHub no navegador

### Use Case 3: Monitorar Repositório Upstream

```
"Listar issues abertas do torvalds/linux que mencionam 
 'memory management' ou 'scheduler'"
```

**Valor:** Acompanhar projeto sem estar no GitHub

---

## 🔗 Referências

### Tools disponíveis no MCP GitHub
- `list_issues` — Listar issues com filtros
- `search_issues` — Busca avançada por query
- `get_issue` — Detalhes de uma issue
- `list_issue_comments` — Comentários de uma issue
- `add_issue_comment` — Criar comentário
- `list_pull_requests` — Listar PRs
- `get_pull_request` — Detalhes de PR
- `create_pull_request` — Criar PR
- `merge_pull_request` — Fazer merge de PR

### Documentação oficial
- https://modelcontextprotocol.io/docs/tools/github
- https://github.com/modelcontextprotocol/server-github

---

## 💡 Tips & Tricks

### Tip 1: Use repositórios públicos para testar
```
❌ Errado: Usar repo privado com permissões limitadas
✅ Certo: Testar com microsoft/vscode ou torvalds/linux
```

### Tip 2: Copilot formata melhor se você fornecer contexto

```
❌ "Liste issues"
✅ "Liste as top 5 issues mais antigas abertas do react"
```

### Tip 3: Aprove sempre antes de write operations

```
Copilot mostra o comentário/PR exato que vai criar
Você revisa e aprova → só depois MCP executa
```

---

**Lab 2 Exemplos Práticos**  
**Criado em:** 17/06/2026
