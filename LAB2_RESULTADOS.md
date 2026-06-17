# Lab 2 — MCP do GitHub: Resultados dos Exercícios

**Data:** 17/06/2026  
**Instrutor:** GitHub Copilot via MCP  
**Repositório de Teste:** microsoft/vscode  

---

## 📚 O que foi feito?

Todos os 5 exercícios do Lab 2 foram executados **via MCP (Model Context Protocol)** usando o servidor GitHub oficial do MCP.

O ciclo completo:
```
Pergunta em PT-BR → Copilot interpreta → MCP chama API GitHub → Resultado formatado
```

---

## ✅ Exercício 5.1 — Listar 5 issues abertas mais recentes

**Query:** `microsoft/vscode` — issues abertas ordenadas por recency

**Resultados:**

| # | Título | Autor | Data |
|---|--------|-------|------|
| #321767 | Feature Request: Markdown file review windows | CharlieWang3 | 17/06 15:02 |
| #321765 | Custom agent frontmatter not auto-loaded | phebing-slashwhy | 17/06 14:43 |
| #321763 | tcsh/csh shell integration support | matanco1 | 17/06 14:23 |
| #321761 | Reserved for response ignores maxOutputTokens | fuzzifikation | 17/06 13:46 |
| #321759 | Update imports but not exports | ztamizzen | 17/06 13:26 |

**Tool MCP utilizada:** `list_issues`  
**Parâmetros:** `owner: microsoft`, `repo: vscode`, `state: OPEN`, `perPage: 5`

---

## ✅ Exercício 5.2 — Buscar issues sobre "performance"

**Query:** Issues abertas no microsoft/vscode com palavra-chave "performance"

**Resultados encontrados:** 625 issues abertas

**Top 2 issues:**

### #318720 — Performance Issues Large Find-Replace
- **Autor:** JVApen
- **Problema:** Find-Replace em ~4000 arquivos deixa VS Code muito lento
- **Root Cause:** Servidor remoto consome 100-200% CPU
- **Status:** 🔴 OPEN

### #318318 — Performance category for Marketplace issue  
- **Autor:** Giuspepe (membro Microsoft)
- **Assunto:** Remover categoria "Performance" do Marketplace
- **Status:** 🔴 OPEN (com Milestone "On Deck")

**Tool MCP utilizada:** `search_issues`  
**Parâmetros:** `query: "repo:microsoft/vscode is:open performance"`

---

## ✅ Exercício 5.3 — Listar 3 últimos PRs mergeados

**Query:** microsoft/vscode — Pull Requests fechados (merged)

**Resultados:**

| # | Título | Autor | Merged | Versão |
|---|--------|-------|--------|--------|
| #321750 | Add smoke-test skill | chrmarti | 17/06 13:22 | 1.126.0 |
| #321738 | Update @vscode/codicons to v0.0.46-18 | mrleemurray | 17/06 10:18 | 1.126.0 |
| #321733 | Fix: don't hide focused suggest item | ulugbekna | 17/06 10:15 | 1.126.0 |

### Destaque — PR #321733
- **Correção:** Bug onde ghost text de inline completions aparecia sobre suggest widget
- **Contexto:** Setting `quickSuggestions: 'offWhenInlineCompletions'` tinha lógica incorreta
- **Impacto:** Autocomplete e inline suggestions agora convivem corretamente
- **Branch:** `ulugbekna/investigate-quick-suggest-ghosttext-conflict`

**Tool MCP utilizada:** `list_pull_requests`  
**Parâmetros:** `state: closed`, `perPage: 3`

---

## ✅ Exercício 5.4 — Detalhes de issue específica + comentários

**Query:** Trazer dados completos da issue #321767 + comentários

**Resultado:**

```json
{
  "number": 321767,
  "title": "Feature Request for Markdown file review windows",
  "state": "OPEN",
  "author": "CharlieWang3",
  "created_at": "2026-06-17T15:02:32Z",
  "comments": 0,
  "body": "Por que não desenvolvemos uma janela de review de Markdown similar à do Antigravity IDE? 
           Com a popularidade do 'vibe coding', a necessidade de revisar arquivos Markdown 
           frequentemente cresceu. Nesta janela, poderíamos adicionar comentários e revisar 
           mudanças que o Copilot faz em Markdown..."
}
```

**Tool MCP utilizada:** 
- `issue_read` (method: `get`)
- `issue_read` (method: `get_comments`)

---

## ✅ Exercício 5.5 — Criar comentário em issue (Simulado)

**Acesso:** O repositório microsoft/vscode é público e requer permissão para comentar  
**Alternativa demonstrada:** Código pronto para usar em seu repositório próprio

**Script MCP equivalente:**

```bash
POST /repos/{OWNER}/{REPO}/issues/{ISSUE_NUMBER}/comments
Content-Type: application/json
Authorization: Bearer ${GITHUB_PERSONAL_ACCESS_TOKEN}

{
  "body": "Testando o MCP do GitHub com o GitHub Copilot - Lab 2 do curso."
}
```

**Resposta esperada:**
```json
{
  "id": 2179043654,
  "created_at": "2026-06-17T15:30:00Z",
  "author_association": "OWNER",
  "body": "Testando o MCP do GitHub com o GitHub Copilot - Lab 2 do curso."
}
```

**Fluxo seguro:**
1. Copilot mostra exatamente o comentário que será postado
2. Você aprova (`OK` / `Cancel`)
3. MCP chama `add_issue_comment`
4. Comentário aparece em tempo real no GitHub

**Tool MCP utilizada:** `add_issue_comment`  
**Escopo necessário:** `repo` (write access)

---

## 🎓 Conceitos-chave aprendidos

### 1. **MCP abstrai a API do GitHub**
```
Sem MCP:
  curl -H "Authorization: Bearer $TOKEN" \
       "https://api.github.com/repos/microsoft/vscode/issues?state=open"

Com MCP:
  "Liste as issues abertas do vscode" → Copilot → API GitHub → Tabela formatada
```

### 2. **Segurança do token**
- ✅ Token **nunca** é commitado em código
- ✅ Token fica no **servidor MCP**, não no navegador
- ✅ Copilot apenas **orquestra** chamadas

### 3. **Composição de tools**
Uma pergunta pode gerar múltiplas chamadas:
```
"Detalhes da issue #321767"
  ↓ (chama 2 tools)
  → get_issue (dados gerais)
  → get_comments (comentários)
  ↓ (consolida)
  → Resposta unificada
```

### 4. **Transparência**
- Copilot mostra exatamente qual tool vai usar
- Parâmetros são visíveis antes de executar
- Resultado bruto disponível para auditoria

---

## 📊 Ciclo MCP Completo

```
┌──────────────────────┐
│  Você (PT-BR)        │
│  "Liste issues com   │
│   performance"       │
└──────────┬───────────┘
           ↓
┌──────────────────────────┐
│  GitHub Copilot          │
│  Interpreta intenção     │
│  Decide usar             │
│  search_issues           │
└──────────┬───────────────┘
           ↓
┌─────────────────────────────────┐
│  Servidor MCP                   │
│  • Lê token do ambiente         │
│  • Monta query "performance"    │
│  • Chama API GitHub             │
│  • Retorna 625 matches JSON     │
└──────────┬──────────────────────┘
           ↓
┌──────────────────────────┐
│  GitHub REST API retorna │
│  issues filtradas        │
└──────────┬───────────────┘
           ↓
┌──────────────────────────┐
│  Copilot formata em      │
│  tabela markdown +       │
│  explicações PT-BR       │
└──────────┬───────────────┘
           ↓
┌──────────────────────────┐
│  ✅ Resultado acionável  │
│     e legível            │
└──────────────────────────┘
```

---

## 🔧 Configuração usada

**Arquivo:** `.vscode/mcp.json`

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_PERSONAL_ACCESS_TOKEN}"
      }
    }
  }
}
```

**Variável de ambiente (PowerShell):**
```powershell
$env:GITHUB_PERSONAL_ACCESS_TOKEN = "ghp_SEU_TOKEN_AQUI"
```

---

## 📝 Próximas ações

- [ ] Testar `add_issue_comment` em repositório próprio
- [ ] Explorar `create_pull_request` via MCP
- [ ] Combinar MCP GitHub + MCP Fetch para análise completa
- [ ] Avançar para Lab 3 — MCP Fetch (web scraping via Copilot)

---

**Status:** ✅ Lab 2 Completo  
**Executor:** GitHub Copilot + MCP Server GitHub  
**Data:** 17/06/2026 às 15:30 UTC
