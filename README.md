# Treinamento GitHub Copilot — Lab 2 Completo

Repositório com resultados práticos do **Lab 2 — MCP do GitHub: casos reais de desenvolvimento**.

## 📁 Estrutura

```
.
├── LAB2_RESULTADOS.md      ← Resultados dos 5 exercícios executados
├── exemplos_mcp_github.md  ← Exemplos práticos de queries e code snippets
└── README.md               ← Este arquivo
```

## 🚀 O que é este Lab?

**Objetivo:** Demonstrar como o GitHub Copilot usa MCP (Model Context Protocol) para interagir com repositórios do GitHub em linguagem natural — sem usar a CLI do GitHub, sem abrir o navegador.

**Impacto:** O Copilot age diretamente no GitHub real, não em simulações.

## ✅ Exercícios Realizados

1. **Listar 5 issues abertas mais recentes** — microsoft/vscode
2. **Buscar issues por tema** — 625 issues com palavra-chave "performance"
3. **Listar 3 últimos PRs mergeados** — com detalhes completos
4. **Trazer dados completos de uma issue** — incluindo comentários
5. **Criar comentário em issue** — código pronto para usar

## 🔍 Highlights

- ✅ **625 issues** encontradas e filtradas automaticamente
- ✅ **3 PRs recentes** analisados com contexto completo
- ✅ **Ciclo MCP** demonstrado passo-a-passo (pergunta → tool call → resultado)
- ✅ **Segurança** — token nunca é exposto, fica no servidor MCP

## 📚 Como usar

### Arquivo principal
Abra [LAB2_RESULTADOS.md](./LAB2_RESULTADOS.md) para ver:
- Resultados de cada exercício em tabelas
- Queries MCP executadas
- Ciclo completo MCP explicado
- Configuração necessária

### Exemplos práticos
Veja [exemplos_mcp_github.md](./exemplos_mcp_github.md) para:
- Code snippets prontos para usar
- Padrões comuns de queries
- Como criar comentários em issues
- Como buscar PR específicas

## 🎓 Conceitos-chave

### MCP (Model Context Protocol)
```
Sua pergunta em PT-BR
       ↓
Copilot interpreta
       ↓
MCP Server GitHub chama API
       ↓
Resultado formatado em tabela/resumo
```

### Por que MCP é poderoso?
- ✅ Abstrai complexidade de APIs
- ✅ Torna prompts mais naturais
- ✅ Compõe múltiplas chamadas automaticamente
- ✅ Mantém segurança do token

## 🔧 Pré-requisitos

- VS Code com GitHub Copilot
- Node.js 18+
- GitHub Personal Access Token (escopo `repo` + `read:org`)

## 📝 Configuração

Arquivo `.vscode/mcp.json`:
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

Variável de ambiente (PowerShell):
```powershell
$env:GITHUB_PERSONAL_ACCESS_TOKEN = "ghp_SEU_TOKEN"
```

## 🎯 Próximos Labs

- **Lab 3:** MCP Fetch — web scraping via Copilot
- **Lab 4:** MCP SQLite — queries de banco de dados
- **Lab 1:** MCP Filesystem — manipulação de arquivos

## 📞 Referências

- [MCP Documentação Oficial](https://modelcontextprotocol.io)
- [GitHub MCP Server](https://github.com/modelcontextprotocol/server-github)
- [Lab 2 Original — Guia Completo](../lab2_github/lab2_github.md)

---

**Executor:** GitHub Copilot + MCP Server GitHub  
**Data:** 17/06/2026  
**Status:** ✅ Completo
