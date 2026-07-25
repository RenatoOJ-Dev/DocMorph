# Sistema Multi-Agente para Conversão de PDF em Markdown Estruturado

**Versão:** 1.0  
**Última atualização:** Junho 2026  
**Status:** Planejamento

---

## 📋 Visão Geral

### Objetivo Principal
Criar um pipeline especializado onde múltiplos modelos de LLM trabalham de forma coordenada para analisar arquivos PDF e gerar documentação Markdown (.md) altamente estruturada, preservando a hierarquia visual, formatação e elementos especiais do documento original.

### Problema que Resolve
Converter PDFs (manuais técnicos, documentação, relatórios, artigos) em Markdown estruturado mantendo fidelidade estrutural, hierarquia de conteúdo, tabelas, blocos de código e legibilidade, gerando um único arquivo .md contínuo e pronto para uso.

---



---

## 📁 Especificação Operacional (Adicionado na Revisão 1.1)

### Estrutura obrigatória

```text
docs/
├── pdf/
└── md/
```

- A pasta raiz deve ser `docs`, `doc` ou `documents`.
- PDFs devem estar em diretórios contendo `pdf` (ex.: `pdf`, `pdf_input`, `meu_pdf`).
- Markdowns devem ser gerados em diretórios contendo `md`.

### Contrato de Entrada

- O agente processa apenas o PDF explicitamente informado.
- Deve procurar recursivamente apenas dentro de `docs`.

### Contrato de Saída

- Cada PDF gera exatamente um Markdown.
- O Markdown deve ser salvo em `docs/md`.
- O nome deve indicar conversão, por exemplo `manual_convertido.md`.
- Imagens extraídas devem ser exportadas para `assets/`.

### Regras Imutáveis

- Nunca alterar o PDF original.
- Nunca mover ou renomear PDFs automaticamente.
- Perguntar antes de sobrescrever um Markdown existente.
- Criar `docs/md` caso não exista.
- O processamento termina somente após todos os agentes aprovarem o resultado e o Markdown ser criado.

### Fluxo Operacional

1. Receber PDF explicitamente indicado.
2. Validar localização.
3. Extrair conteúdo e imagens.
4. Analisar estrutura.
5. Planejar Markdown.
6. Gerar Markdown.
7. Validar.
8. Exportar imagens.
9. Salvar em docs/md.


## 🏗️ Arquitetura do Time

### Agente 1: Extrator de Conteúdo PDF

**Responsabilidades:**
- Ler e extrair todo o conteúdo textual do PDF
- Identificar codificação e idioma do documento
- Extrair metadata do PDF (título, autor, data de criação, número de páginas)
- Detectar se o PDF é baseado em texto ou imagem (OCR necessário)
- Preservar quebras de linha e parágrafos originais
- Identificar caracteres especiais e símbolos
- Extrair imagens embutidas (quando aplicável)

**Output Esperado:**

```json
{
  "metadata_pdf": {
    "titulo": "",
    "autor": "",
    "data_criacao": "",
    "numero_paginas": 0,
    "tamanho_kb": 0,
    "tipo": "texto/imagem/misto"
  },
  "conteudo_bruto": {
    "texto_completo": "",
    "quebras_pagina": [],
    "idioma_detectado": "",
    "codificacao": ""
  },
  "elementos_especiais_detectados": {
    "imagens": 0,
    "tabelas": 0,
    "blocos_codigo": 0,
    "listas": 0
  }
}
```

---

### Agente 2: Analista de Estrutura Hierárquica

**Responsabilidades:**
- Identificar hierarquia de títulos (H1, H2, H3, H4, H5, H6)
- Detectar seções e subseções
- Mapear estrutura de tópicos e sumário
- Identificar listas (numeradas, bullet points, multi-nível)
- Detectar blocos de código e exemplos
- Identificar citações e callouts
- Mapear referências cruzadas e links internos
- Preservar ordem lógica do conteúdo

**Output Esperado:**

```json
{
  "estrutura_hierarquica": [
    {
      "nivel": 1,
      "titulo": "",
      "pagina": 0,
      "subsecoes": []
    }
  ],
  "elementos_estruturais": {
    "listas": [
      {
        "tipo": "numerada/bullet/mista",
        "nivel_profundidade": 0,
        "localizacao_pagina": 0
      }
    ],
    "blocos_codigo": [
      {
        "linguagem": "",
        "pagina": 0,
        "tamanho_linhas": 0
      }
    ],
    "citacoes": [],
    "referencias_cruzadas": []
  },
  "sumario_detectado": {
    "presente": true,
    "pagina": 0,
    "estrutura": []
  }
}
```

---

### Agente 3: Analista de Elementos Especiais

**Responsabilidades:**
- Extrair e analisar tabelas (estrutura, cabeçalhos, dados)
- Identificar blocos de código com linguagem de programação
- Detectar diagramas e fluxogramas (converter para ASCII ou descrição)
- Identificar fórmulas matemáticas (converter para LaTeX ou descrição)
- Mapear imagens e gerar descrições alternativas
- Detectar notas de rodapé e referências
- Identificar caixas de destaque, alertas e callouts

**Output Esperado:**

```json
{
  "tabelas": [
    {
      "id": "tabela_1",
      "pagina": 0,
      "dimensoes": {"linhas": 0, "colunas": 0},
      "cabecalhos": [],
      "estrutura": [],
      "complexidade": "simples/complexa/aninhada"
    }
  ],
  "blocos_codigo": [
    {
      "id": "code_1",
      "pagina": 0,
      "linguagem": "",
      "conteudo": "",
      "linhas": 0
    }
  ],
  "imagens": [
    {
      "id": "img_1",
      "pagina": 0,
      "descricao_alternativa": "",
      "tipo": "foto/ilustracao/diagrama"
    }
  ],
  "notas_rodape": [],
  "callouts": []
}
```

---

### Agente 4: Arquiteto de Markdown

**Responsabilidades:**
- Planejar estrutura do arquivo .md final
- Definir hierarquia de headers (H1-H6)
- Planejar uso de tabelas Markdown
- Definir estrutura de code blocks (com syntax highlighting)
- Planejar diagramas ASCII para fluxos quando necessário
- Definir uso de callouts e blockquotes
- Mapear índices e âncoras para navegação
- Garantir que o output será um único arquivo contínuo

**Output Esperado:**

```json
{
  "estrutura_md_planejada": {
    "cabecalho": {
      "titulo_principal": "",
      "metadata": ["versao", "data", "status"]
    },
    "secoes_principais": [
      {
        "titulo": "",
        "nivel_header": 2,
        "subsecoes": []
      }
    ]
  },
  "elementos_md_utilizados": {
    "tabelas": 0,
    "code_blocks": 0,
    "diagramas_ascii": 0,
    "blockquotes": 0,
    "listas": 0
  },
  "estilo_formatacao": {
    "usar_tabelas_dados": true,
    "usar_code_blocks": true,
    "usar_diagramas_ascii": false,
    "arquivo_unico_continuo": true
  }
}
```

---

### Agente 5: Desenvolvedor Markdown

**Responsabilidades:**
- Escrever o arquivo .md completo baseado na arquitetura do Agente 4
- Aplicar formatação Markdown correta
- Implementar tabelas bem formatadas
- Criar code blocks com syntax highlighting
- Aplicar hierarquia visual clara
- Adicionar metadata no cabeçalho
- Criar índices e navegação quando apropriado
- Garantir que todo o conteúdo esteja em UM ÚNICO arquivo contínuo
- Não dividir em múltiplos blocos de código separados

**Output Esperado:**
- Arquivo .md completo e formatado
- Estrutura seguindo padrão de referência
- Todos os elementos especiais convertidos
- Metadata completa no topo
- Documento único e contínuo

---

### Agente 6: Validador de Fidelidade Estrutural

**Responsabilidades:**
- Comparar PDF original vs .md gerado
- Verificar se toda hierarquia foi preservada
- Validar se tabelas estão completas e corretas
- Confirmar se code blocks estão formatados corretamente
- Verificar se listas mantêm estrutura original
- Validar formatação (negritos, itálicos, etc.)
- Testar renderização do Markdown
- Verificar links e referências
- Confirmar que é um único arquivo contínuo

**Output Esperado:**

```json
{
  "validacao_estrutura": {
    "hierarquia_preservada": true,
    "secoes_completas": true,
    "subsecoes_completas": true
  },
  "validacao_elementos": {
    "tabelas_completas": true,
    "code_blocks_corretos": true,
    "listas_estruturadas": true,
    "links_funcionais": true
  },
  "validacao_formatacao": {
    "destaques_preservados": true,
    "hierarquia_visual_clara": true,
    "arquivo_unico_continuo": true
  },
  "score_fidelidade_geral": 0,
  "problemas_encontrados": [],
  "recomendacoes_correcao": []
}
```

---

### Agente 7: Otimizador e Formatador Final

**Responsabilidades:**
- Otimizar formatação do .md
- Garantir consistência visual
- Adicionar separadores (---) onde apropriado
- Otimizar tamanhos de tabelas
- Melhorar legibilidade
- Garantir compatibilidade com renderizadores Markdown
- Criar versão final limpa e contínua
- Confirmar que o documento é um único arquivo pronto para salvar

**Output Esperado:**
- Arquivo .md otimizado e final
- Relatório de otimizações aplicadas
- Versão final pronta para uso
- Documento único e contínuo

---

## 🔄 Fluxo de Trabalho (Algoritmo Completo)

```
┌─────────────────────────────────────────────────────────────┐
│                    ARQUIVO PDF ORIGINAL                     │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 1: EXTRATOR DE CONTEÚDO PDF                         │
│  → Leitura completa do PDF                                  │
│  → Extração de metadata                                     │
│  → Detecção de tipo (texto/imagem)                          │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 2: ANALISTA DE ESTRUTURA                            │
│  → Mapeamento de hierarquia                                 │
│  → Identificação de seções/subseções                        │
│  → Detecção de listas e blocos de código                    │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 3: ANALISTA DE ELEMENTOS ESPECIAIS                  │
│  → Extração de tabelas                                      │
│  → Identificação de code blocks                             │
│  → Detecção de imagens e diagramas                          │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 4: ARQUITETO DE MARKDOWN                            │
│  → Planejamento da estrutura .md                            │
│  → Definição de hierarquia                                  │
│  → Mapeamento de elementos Markdown                         │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
         ┌──────────────────────┐
         │ Estrutura aprovada?  │
         └──┬───────────────┬───┘
            │ SIM           │ NÃO
            ↓               ↓
┌─────────────────┐    ┌──────────────────────────┐
│ AGENTE 5        │    │ RETORNO PARA              │
│ DESENVOLVEDOR   │    │ AGENTE 4                  │
│ → Escrita .md   │    │ → Replanejamento          │
│ → Formatação    │    └──────────────────────────┘
└────────┬────────┘
         ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 6: VALIDADOR DE FIDELIDADE                          │
│  → Comparação PDF vs .md                                    │
│  → Verificação de completude                                │
│  → Score de fidelidade                                      │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
         ┌──────────────────────┐
         │ Score ≥ 85?          │
         └──┬───────────────┬───┘
            │ SIM           │ NÃO
            ↓               ↓
┌─────────────────┐    ┌──────────────────────────┐
│ AGENTE 7        │    │ RETORNO ITERATIVO:       │
│ OTIMIZADOR      │    │ → Score 70-84: Agente 5  │
│ → Formatação    │    │ → Score < 70: Agente 4   │
│ → Versão final  │    │ → Correções              │
└────────┬────────┘    └──────────────────────────┘
         ↓
┌─────────────────────────────────────────────────────────────┐
│   ✅ ARQUIVO .MD FINAL                                      │
│                                                              │
│  • Estruturado seguindo padrão de referência                │
│  • Com tabelas, code blocks, listas                         │
│  • Metadata completa                                        │
│  • Arquivo único e contínuo                                 │
│  • Pronto para salvar e usar                                │
└─────────────────────────────────────────────────────────────┘
```

---

## ✅ Critérios de Aceitação

### Obrigatórios (todos devem ser atendidos):
- ✅ **Estrutura Hierárquica**: Toda a hierarquia de títulos e seções deve ser preservada
- ✅ **Tabelas Completas**: Todas as tabelas do PDF devem estar presentes no .md
- ✅ **Code Blocks**: Blocos de código devem estar formatados com syntax highlighting
- ✅ **Fidelidade Estrutural**: Mínimo 85% de similaridade estrutural com o PDF original
- ✅ **Legibilidade**: O .md deve ser legível e bem formatado em qualquer renderizador Markdown
- ✅ **Arquivo Único**: O output deve ser um único arquivo .md contínuo, não dividido em múltiplos blocos

### Desejáveis:
- ⭐ **Índices e Navegação**: Links internos para navegação rápida
- ⭐ **Checklists**: Listas de verificação formatadas como checkboxes
- ⭐ **Comentários HTML**: Comentários explicativos quando necessário
- ⭐ **Metadata Completa**: Informações detalhadas no cabeçalho

---

## 📦 Formato de Entrega Final

```json
{
  "metadata": {
    "pdf_original": {
      "titulo": "",
      "autor": "",
      "paginas": 0,
      "tamanho_kb": 0
    },
    "processamento": {
      "iteracoes": 0,
      "agentes_envolvidos": [],
      "tempo_estimado": ""
    }
  },
  "markdown_gerado": {
    "conteudo_completo": "# ...",
    "tamanho_kb": 0,
    "numero_secoes": 0,
    "numero_tabelas": 0,
    "numero_code_blocks": 0,
    "arquivo_unico": true
  },
  "validacoes": {
    "estrutura_preservada": true,
    "tabelas_completas": true,
    "formatacao_correta": true,
    "score_fidelidade_geral": 0
  },
  "elementos_convertidos": {
    "titulos": 0,
    "subtitulos": 0,
    "tabelas": 0,
    "code_blocks": 0,
    "listas": 0,
    "imagens_descritas": 0
  },
  "status_final": "APROVADO/REPROVADO",
  "observacoes": ""
}
```

---

## 🔁 Mecanismo de Feedback e Iteração

### Níveis de Retorno:

1. **Retorno Leve (Agente 7 → Agente 6)**
   - Ajustes menores de formatação
   - Correção de espaçamentos
   - Melhorias de legibilidade

2. **Retorno Médio (Agente 6 → Agente 5)**
   - Score 70-84
   - Correção de elementos faltantes
   - Ajustes de formatação Markdown

3. **Retorno Pesado (Agente 6 → Agente 4)**
   - Score < 70
   - Replanejamento da estrutura
   - Revisão da arquitetura Markdown

4. **Retorno Crítico (Qualquer agente → Agente 2)**
   - Estrutura hierárquica incorreta
   - Reanálise completa do PDF

### Limite de Iterações:
- **Máximo:** 3 iterações por PDF
- **Após 3 iterações sem aprovação:** Escalar para revisão humana
- **Cada iteração deve melhorar score em mínimo 5 pontos**

---

## 🛠️ Ferramentas e Recursos Recomendados

### Para Extração de PDF:
- **PyMuPDF (fitz)** - Extração de texto e imagens
- **pdfplumber** - Extração de tabelas
- **pdf2image** - Conversão de páginas em imagens
- **Tesseract OCR** - Para PDFs baseados em imagem

### Para Análise e Processamento:
- **spaCy** - Análise de estrutura textual
- **Pandas** - Manipulação de dados de tabelas

### Para Geração de Markdown:
- **markdown-it** - Renderização e validação
- **mdformat** - Formatação automática
- **markdownlint** - Linting de Markdown

### Bibliotecas Úteis:
- **tabulate** - Formatação de tabelas
- **pygments** - Syntax highlighting para code blocks

---

## 📊 Métricas de Performance

| Métrica | Meta | Crítico |
|---------|------|---------|
| Fidelidade Estrutural | ≥ 90% | < 70% |
| Score Geral | ≥ 85 | < 60 |
| Tabelas Completas | 100% | < 90% |
| Tempo de Processamento | < 60s/página | > 3min/página |
| Iterações | ≤ 2 | > 3 |
| Arquivo Único | Sim | Não |

---

## 🎯 Casos de Uso

### Ideal Para:
- ✅ Manuais técnicos e documentação
- ✅ Relatórios corporativos
- ✅ Artigos acadêmicos
- ✅ Especificações de projeto
- ✅ Documentação de APIs
- ✅ Apresentações convertidas em docs

### Não Recomendado Para:
- ❌ PDFs com muitas imagens fotográficas
- ❌ Documentos com layouts complexos (revistas)
- ❌ PDFs escaneados de baixa qualidade
- ❌ Documentos com formulários interativos complexos
- ❌ PDFs com proteção contra cópia

---

## 📝 Notas de Implementação

### Requisitos Técnicos:
- Modelos LLM com capacidade de análise de documentos
- Suporte a extração de PDF (texto e imagens)
- Capacidade de geração de Markdown estruturado
- Sistema de orquestração de agentes
- Ferramentas de OCR para PDFs baseados em imagem

### Boas Práticas:
- Sempre validar a extração do PDF antes do processamento
- Manter versão original do PDF para referência
- Testar renderização em múltiplos visualizadores Markdown
- Documentar decisões de conversão
- Considerar acessibilidade na estrutura Markdown
- Garantir que o output seja um único arquivo contínuo

### Desafios Comuns:
- **Tabelas complexas:** Podem exigir múltiplas iterações
- **Diagramas:** Conversão para ASCII art pode ser desafiadora
- **Formatação visual:** Nem todos os estilos são traduzíveis para Markdown
- **Imagens:** Requerem descrição textual

---

## 🔄 Versionamento

| Versão | Data | Mudanças |
|--------|------|----------|
| 1.0 | Junho 2026 | Versão inicial com 7 agentes |

---
**Autor:** Sistema Multi-Agente Conversão PDF para Markdown  
**Última revisão:** Junho 2026
