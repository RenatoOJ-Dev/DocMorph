# DocMorph

DocMorph é um sistema multi-agente para transformação inteligente de documentos.

O projeto foi criado para resolver um problema comum: documentos PDF são ótimos para distribuição, mas difíceis de editar, enquanto arquivos Markdown são fáceis de modificar, mas não preservam a aparência visual do documento original.

O DocMorph atua nos dois sentidos:

* PDF → Markdown estruturado
* Markdown → PDF formatado

Através de uma arquitetura baseada em agentes especializados, o sistema é capaz de analisar documentos, identificar sua estrutura hierárquica, extrair padrões visuais reutilizáveis e reconstruir documentos mantendo organização, legibilidade e fidelidade estrutural.

## Funcionalidades

* Conversão de PDF para Markdown estruturado
* Conversão de Markdown para PDF formatado
* Reconhecimento de templates visuais
* Extração de hierarquia documental
* Preservação de títulos, seções e subseções
* Reconstrução de layouts
* Sistema de validação estrutural
* Arquitetura multi-agente

## Fluxos Principais

### from_pdf_to_md

Responsável por analisar documentos PDF e gerar arquivos Markdown organizados e editáveis.

Fluxo:

PDF → Estrutura → Markdown

### from_md_to_pdf

Responsável por transformar arquivos Markdown em documentos PDF formatados.

Fluxo:

Markdown → Template Visual → PDF

## Casos de Uso

* Currículos
* Relatórios
* Apostilas
* Documentação técnica
* Manuais
* Artigos
* Portfólios

## Objetivo

Transformar documentos estáticos em estruturas reutilizáveis e inteligentes, permitindo edição, reorganização e reconstrução sem perder a identidade visual do documento original.

## Authors

* Renato (@RenatoOJ-Dev)
* ChatGPT (OpenAI) 
* Qwen (Alibaba Cloud)

