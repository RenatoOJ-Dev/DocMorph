# Sistema Multi-Agente para Conversão de Markdown em PDF Formatado

**Versão:** 1.0  
**Última atualização:** Junho 2026  
**Status:** Planejamento  
**Fluxo:** `from_md_to_pdf`

---

## 📋 Visão Geral

### Objetivo Principal

Criar um pipeline especializado onde múltiplos agentes trabalham de forma coordenada para analisar arquivos Markdown (`.md`) já estruturados, aplicar ajustes solicitados pelo usuário e gerar um arquivo PDF final com formatação visual organizada, legível e fiel a um template de referência.

### Problema que Resolve

Transformar um Markdown editável em um PDF final pronto para uso, mantendo:

- Hierarquia visual clara;
- Seções bem organizadas;
- Títulos e subtítulos destacados;
- Blocos repetitivos bem formatados;
- Paginação adequada;
- Aparência próxima ao PDF modelo, quando existir;
- Ajustes personalizados pedidos pelo usuário.

---

## 🎯 Ideia Central

Este sistema é o fluxo inverso do `from_pdf_to_md`.

Enquanto o `from_pdf_to_md` faz:

```text
PDF original
↓
Extração de estrutura
↓
Markdown estruturado
```

este fluxo faz:

```text
Markdown estruturado
↓
Aplicação de ajustes
↓
Template visual
↓
PDF final formatado
```

A lógica principal é:

```text
Markdown = conteúdo lógico e editável
Template/PDF modelo = aparência visual
PDF final = conteúdo do Markdown + formatação visual
```

---

## 🏗️ Arquitetura do Time

### Agente 1: Leitor de Markdown

**Responsabilidades:**

- Ler o arquivo `.md` completo.
- Identificar título principal.
- Identificar seções e subseções.
- Detectar listas, tabelas, links e blocos de código.
- Detectar blocos repetitivos, como cursos, experiências, projetos ou tópicos.
- Verificar se o Markdown está bem estruturado.
- Separar conteúdo útil de metadados técnicos, quando necessário.

**Output Esperado:**

```json
{
  "markdown_lido": {
    "titulo_principal": "",
    "numero_secoes": 0,
    "numero_subsecoes": 0,
    "listas": 0,
    "tabelas": 0,
    "links": 0,
    "code_blocks": 0
  },
  "estrutura_detectada": [
    {
      "nivel": 1,
      "titulo": "",
      "conteudo": "",
      "subsecoes": []
    }
  ],
  "problemas_detectados": []
}
```

---

### Agente 2: Normalizador de Conteúdo

**Responsabilidades:**

- Corrigir inconsistências simples do Markdown.
- Padronizar títulos.
- Remover excesso de quebras de linha.
- Organizar blocos repetitivos.
- Separar conteúdo de metadados técnicos.
- Garantir que o documento esteja pronto para renderização.
- Manter o conteúdo original, exceto quando o usuário pedir alteração.

**Exemplos de normalização:**

```text
Antes:
**Duração:** 79.00

Depois:
Duração: 79 horas
```

```text
Antes:
## Relatório de conversão

Depois:
Remover, caso o PDF final seja um currículo ou documento de entrega.
```

**Output Esperado:**

```json
{
  "conteudo_normalizado": true,
  "alteracoes_aplicadas": [],
  "conteudo_removido": [],
  "observacoes": ""
}
```

---

### Agente 3: Aplicador de Instruções do Usuário

**Responsabilidades:**

- Ler pedidos específicos do usuário.
- Aplicar alterações de conteúdo autorizadas.
- Aplicar alterações visuais solicitadas.
- Adaptar o documento para objetivo específico, quando pedido.
- Preservar tudo que o usuário não pediu para alterar.

**Tipos de pedidos aceitos:**

- Adicionar curso;
- Remover curso;
- Reordenar seções;
- Deixar mais resumido;
- Deixar mais profissional;
- Adaptar para uma vaga;
- Usar apenas uma página;
- Usar duas páginas;
- Alterar cor;
- Alterar fonte;
- Alterar espaçamento;
- Usar o mesmo estilo do PDF modelo.

**Regra principal:**

```text
O agente só altera conteúdo quando o usuário autorizar.
```

**Output Esperado:**

```json
{
  "instrucoes_usuario": [],
  "alteracoes_conteudo": [],
  "alteracoes_visuais": [],
  "conteudo_preservado": true
}
```

---

### Agente 4: Interpretador de Template Visual

**Responsabilidades:**

- Ler um PDF modelo, quando fornecido.
- Extrair o padrão visual do documento.
- Identificar margens, fontes, espaçamentos e hierarquia visual.
- Detectar rodapés, cabeçalhos e separadores.
- Identificar estilo de títulos, seções e blocos.
- Criar uma representação reutilizável do template.

**Quando existir PDF modelo:**

```text
PDF modelo = referência visual
Markdown = conteúdo
Usuário = ajustes finais
```

**Output Esperado:**

```json
{
  "template_visual": {
    "tipo_documento": "curriculo/relatorio/manual/artigo/outro",
    "tamanho_pagina": "A4",
    "orientacao": "retrato",
    "margens": {
      "superior": "",
      "inferior": "",
      "esquerda": "",
      "direita": ""
    },
    "fontes": {
      "titulo": "",
      "secao": "",
      "texto": ""
    },
    "cores": [],
    "separadores": true,
    "rodape": true,
    "cabecalho": false
  }
}
```

---

### Agente 5: Arquiteto de Layout PDF

**Responsabilidades:**

- Planejar como o Markdown será distribuído no PDF.
- Definir hierarquia visual.
- Definir quebras de página.
- Evitar títulos isolados no fim da página.
- Evitar blocos cortados de forma ruim.
- Planejar espaçamento entre seções.
- Definir como listas, tabelas e links serão exibidos.
- Adaptar o layout ao tipo de documento.

**Output Esperado:**

```json
{
  "layout_planejado": {
    "paginas_estimadas": 0,
    "secoes": [],
    "quebras_pagina": [],
    "estilo_titulos": "",
    "estilo_blocos": "",
    "rodape": "",
    "observacoes_layout": []
  }
}
```

---

### Agente 6: Desenvolvedor de PDF

**Responsabilidades:**

- Transformar o Markdown em uma representação renderizável.
- Gerar HTML/CSS, LaTeX ou outro formato intermediário.
- Aplicar o template visual.
- Gerar o PDF final.
- Garantir compatibilidade com impressão.
- Garantir que links e textos estejam legíveis.

**Ferramentas possíveis:**

- HTML + CSS + WeasyPrint;
- Pandoc;
- ReportLab;
- LaTeX;
- Playwright/Chromium;
- wkhtmltopdf.

**Output Esperado:**

```json
{
  "pdf_gerado": true,
  "arquivo_saida": "documento_final.pdf",
  "metodo_renderizacao": "",
  "numero_paginas": 0,
  "avisos": []
}
```

---

### Agente 7: Validador Visual e Estrutural

**Responsabilidades:**

- Verificar se o PDF foi gerado corretamente.
- Conferir se todo conteúdo do Markdown aparece no PDF.
- Verificar se não há texto cortado.
- Validar se a paginação está aceitável.
- Comparar com o PDF modelo, se houver.
- Verificar se a hierarquia visual está clara.
- Verificar se o PDF está pronto para envio ou impressão.

**Output Esperado:**

```json
{
  "validacao_pdf": {
    "pdf_gerado": true,
    "conteudo_preservado": true,
    "texto_cortado": false,
    "layout_legivel": true,
    "hierarquia_visual_clara": true,
    "template_respeitado": true
  },
  "score_visual": 0,
  "problemas_encontrados": [],
  "recomendacoes_correcao": []
}
```

---

### Agente 8: Otimizador Final

**Responsabilidades:**

- Ajustar espaçamentos finais.
- Melhorar legibilidade.
- Corrigir pequenas falhas de paginação.
- Otimizar tamanho do PDF.
- Ajustar margens quando necessário.
- Gerar versão final pronta para entrega.

**Output Esperado:**

- PDF final otimizado.
- Relatório de ajustes aplicados.
- Documento pronto para uso.

---

## 🔄 Fluxo de Trabalho

```text
┌─────────────────────────────────────────────────────────────┐
│                    ARQUIVO MARKDOWN                         │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 1: LEITOR DE MARKDOWN                               │
│  → Leitura completa do .md                                  │
│  → Identificação de estrutura                               │
│  → Detecção de seções, listas, links e tabelas              │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 2: NORMALIZADOR DE CONTEÚDO                         │
│  → Padronização do Markdown                                 │
│  → Limpeza de metadados técnicos, quando necessário         │
│  → Organização de blocos repetitivos                        │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 3: APLICADOR DE INSTRUÇÕES DO USUÁRIO               │
│  → Ajustes de conteúdo                                      │
│  → Ajustes visuais                                          │
│  → Adaptação ao objetivo do documento                       │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 4: INTERPRETADOR DE TEMPLATE VISUAL                 │
│  → Leitura de PDF modelo ou template salvo                  │
│  → Extração de estilo visual                                │
│  → Definição de aparência base                              │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 5: ARQUITETO DE LAYOUT PDF                          │
│  → Planejamento de páginas                                  │
│  → Hierarquia visual                                        │
│  → Quebras de página                                        │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
         ┌──────────────────────┐
         │ Layout aprovado?     │
         └──┬───────────────┬───┘
            │ SIM           │ NÃO
            ↓               ↓
┌─────────────────┐    ┌──────────────────────────┐
│ AGENTE 6        │    │ RETORNO PARA              │
│ DESENVOLVEDOR   │    │ AGENTE 5                  │
│ → Geração PDF   │    │ → Replanejamento          │
└────────┬────────┘    └──────────────────────────┘
         ↓
┌─────────────────────────────────────────────────────────────┐
│  AGENTE 7: VALIDADOR VISUAL E ESTRUTURAL                    │
│  → Confere conteúdo                                         │
│  → Confere layout                                           │
│  → Confere paginação                                        │
│  → Score visual                                             │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
         ┌──────────────────────┐
         │ Score ≥ 85?          │
         └──┬───────────────┬───┘
            │ SIM           │ NÃO
            ↓               ↓
┌─────────────────┐    ┌──────────────────────────┐
│ AGENTE 8        │    │ RETORNO ITERATIVO:       │
│ OTIMIZADOR      │    │ → Score 70-84: Agente 6  │
│ → Versão final  │    │ → Score < 70: Agente 5   │
└────────┬────────┘    └──────────────────────────┘
         ↓
┌─────────────────────────────────────────────────────────────┐
│   ✅ ARQUIVO PDF FINAL                                      │
│                                                              │
│  • Formatado                                                │
│  • Legível                                                  │
│  • Paginado corretamente                                    │
│  • Fiel ao template visual                                  │
│  • Pronto para envio, impressão ou publicação               │
└─────────────────────────────────────────────────────────────┘
```

---

## ✅ Critérios de Aceitação

### Obrigatórios

- ✅ **Conteúdo Preservado:** Todo conteúdo do Markdown deve aparecer no PDF, exceto remoções pedidas pelo usuário.
- ✅ **PDF Gerado:** O arquivo final deve ser um `.pdf` válido.
- ✅ **Legibilidade:** O PDF deve ser visualmente claro.
- ✅ **Hierarquia Visual:** Títulos, seções e blocos devem ser distinguíveis.
- ✅ **Paginação Correta:** Não deve haver cortes graves de texto.
- ✅ **Fidelidade Visual:** Se houver PDF modelo, o PDF final deve seguir sua formatação.
- ✅ **Ajustes do Usuário:** Pedidos explícitos devem ser aplicados.
- ✅ **Pronto para Uso:** O PDF deve estar adequado para envio, impressão ou publicação.

### Desejáveis

- ⭐ Links clicáveis.
- ⭐ Rodapé personalizado.
- ⭐ Cabeçalho personalizado.
- ⭐ Sumário automático, quando apropriado.
- ⭐ Numeração de páginas.
- ⭐ Tema visual reutilizável.
- ⭐ Versão compacta e versão detalhada.

---

## 📦 Formato de Entrega Final

```json
{
  "metadata": {
    "markdown_original": {
      "titulo": "",
      "tamanho_kb": 0,
      "numero_secoes": 0
    },
    "template_visual": {
      "usado": true,
      "origem": "pdf_modelo/template_padrao/instrucoes_usuario"
    },
    "processamento": {
      "iteracoes": 0,
      "agentes_envolvidos": [],
      "tempo_estimado": ""
    }
  },
  "pdf_gerado": {
    "arquivo": "documento_final.pdf",
    "numero_paginas": 0,
    "tamanho_kb": 0,
    "formato": "A4",
    "orientacao": "retrato"
  },
  "validacoes": {
    "conteudo_preservado": true,
    "layout_legivel": true,
    "sem_texto_cortado": true,
    "template_respeitado": true,
    "score_visual": 0
  },
  "status_final": "APROVADO/REPROVADO",
  "observacoes": ""
}
```

---

## 🔁 Mecanismo de Feedback e Iteração

### Retorno Leve

Quando o PDF está correto, mas precisa de ajustes pequenos:

- Espaçamento;
- Margens;
- Tamanho de fonte;
- Separadores;
- Rodapé;
- Pequenas quebras de página.

Retorno:

```text
Agente 8 → Agente 7
```

---

### Retorno Médio

Quando o conteúdo está correto, mas o PDF ficou visualmente ruim:

- Página muito cheia;
- Títulos mal posicionados;
- Blocos quebrados;
- Hierarquia pouco clara.

Retorno:

```text
Agente 7 → Agente 6
```

---

### Retorno Pesado

Quando o layout planejado não serve para o documento:

- Conteúdo grande demais;
- Template incompatível;
- Estrutura visual confusa;
- Necessidade de replanejar páginas.

Retorno:

```text
Agente 7 → Agente 5
```

---

### Retorno Crítico

Quando o Markdown de entrada está muito desorganizado:

- Títulos inconsistentes;
- Blocos misturados;
- Seções sem padrão;
- Conteúdo ambíguo.

Retorno:

```text
Qualquer agente → Agente 2
```

---

## 📊 Métricas de Performance

| Métrica | Meta | Crítico |
|---|---:|---:|
| Conteúdo preservado | 100% | < 95% |
| Score visual | ≥ 85 | < 70 |
| Fidelidade ao template | ≥ 85% | < 70% |
| Texto cortado | 0 ocorrências | ≥ 1 grave |
| Tempo de geração | < 60s/página | > 3min/página |
| Iterações | ≤ 2 | > 3 |
| PDF válido | Sim | Não |

---

## 🛠️ Ferramentas Recomendadas

### Para leitura e análise de Markdown

- markdown-it
- mistune
- Python-Markdown
- frontmatter
- markdownlint
- mdformat

### Para geração de PDF

- WeasyPrint
- Pandoc
- ReportLab
- LaTeX
- wkhtmltopdf
- Playwright/Chromium

### Para validação visual

- PyMuPDF
- pdfplumber
- pdf2image
- ImageMagick
- scripts de comparação visual

---

## 🎯 Casos de Uso

### Ideal Para

- ✅ Currículos;
- ✅ Relatórios;
- ✅ Documentação técnica;
- ✅ Apostilas;
- ✅ Manuais;
- ✅ Propostas comerciais;
- ✅ Artigos;
- ✅ Portfólios;
- ✅ Documentos acadêmicos.

### Não Recomendado Para

- ❌ Designs extremamente gráficos;
- ❌ Revistas com layout editorial complexo;
- ❌ PDFs com muitas imagens posicionadas manualmente;
- ❌ Documentos que exigem diagramação profissional exata;
- ❌ Formulários interativos complexos.

---

## 🧩 Modo Especial: Markdown + PDF Modelo

Este é o modo mais importante para manter a formatação original.

Entrada:

```text
conteudo.md
pdf_modelo.pdf
```

Saída:

```text
conteudo_formatado.pdf
```

Regra:

```text
O Markdown fornece o conteúdo.
O PDF modelo fornece a aparência.
```

Exemplo:

```text
curriculo.md
curriculo_modelo.pdf
↓
curriculo_final.pdf
```

---

## 🧾 Template Visual para Currículo

Quando o documento for um currículo, usar este padrão:

```text
Nome no topo
Formação atual abaixo do nome
Contato e localização
Interesses
Perfil comportamental
Competências comportamentais
Valores
Idiomas
Experiências
Formação e Capacitação
Portfólio
```

### Estilo Visual Recomendado

```text
Página: A4
Orientação: Retrato
Fonte: Sans-serif
Cor principal: Cinza escuro
Títulos: Grandes e em negrito
Subtítulos: Médios e em negrito
Texto: Regular
Separadores: Linhas horizontais suaves
Rodapé: Data de geração ou numeração
Espaçamento: Compacto, mas legível
```

### Bloco de Curso

```text
Nome do curso
Principal conhecimento adquirido: ...
Nível: ...
Duração: ...
Instituição: ...
```

### Bloco de Experiência

```text
Cargo
Empresa | Período (Duração)
Principal conhecimento adquirido: ...
```

### Bloco de Portfólio

```text
Nome do projeto
Área: ...
Principais conhecimentos adquiridos:
- ...
- ...
Link: ...
```

---

## 📝 Notas de Implementação

### Regra de Ouro

O sistema nunca deve tratar Markdown como texto simples.

Ele deve tratar Markdown como:

```text
estrutura lógica editável
```

E nunca deve tratar PDF como apenas uma imagem.

Ele deve tratar PDF como:

```text
renderização visual da estrutura
```

### Diferença entre Conteúdo e Aparência

```text
Conteúdo = vem do Markdown
Aparência = vem do template visual
Ajustes = vêm do usuário
```

### Ordem de prioridade

```text
1. Pedido explícito do usuário
2. Conteúdo do Markdown
3. Template visual
4. Boas práticas de legibilidade
```

---

## 🚨 Regras de Segurança e Fidelidade

- Não inventar experiências, cursos ou dados.
- Não remover conteúdo sem pedido do usuário.
- Não resumir automaticamente sem autorização.
- Não alterar datas sem autorização.
- Não mudar nomes próprios.
- Não mudar links.
- Não ocultar informação importante.
- Não gerar PDF sem validação visual mínima.

---

## 🔄 Versionamento

| Versão | Data | Mudanças |
|---|---|---|
| 1.0 | Junho 2026 | Versão inicial do fluxo from_md_to_pdf |

---

**Licença:** MIT  
**Autor:** Sistema Multi-Agente Markdown para PDF  
**Última revisão:** Junho 2026
