---
name: write-kanban-card
description: Crie textos simples e objetivos para cards genéricos de tarefas Kanban, sempre com título, os tópicos “O que”, “Por quê” e “Critérios de aceite”. Use quando o usuário pedir para escrever, rascunhar, estruturar ou transformar uma necessidade, funcionalidade, correção ou melhoria em um card de tarefa Kanban.
---

# Write Kanban Card

Transforme a solicitação do usuário em um único texto pronto para copiar em um card de tarefa Kanban.

## Procedimento

1. Identifique a tarefa, o resultado esperado e o contexto fornecido pelo usuário.
2. Se faltarem informações essenciais para descrever a tarefa ou definir critérios verificáveis, faça perguntas objetivas antes de gerar o card. Considere essenciais: o objetivo da tarefa, o motivo de sua existência e o resultado mínimo esperado.
3. Crie um título curto, específico e orientado à ação. Não use “Card”, “Tarefa” ou placeholders no título.
4. Escreva o card exclusivamente com título e os três tópicos obrigatórios.

## Regras de conteúdo

- Inclua sempre, com esta grafia exata, os rótulos `O que:`, `Por quê:` e `Critérios de aceite:`.
- Mantenha o texto em português, salvo solicitação explícita em outro idioma.
- Faça o título refletir o resultado da tarefa, não apenas o tema geral.
- Escreva critérios de aceite como condições de conclusão, usando linguagem objetiva e testável. Inclua todos os critérios mínimos identificados na solicitação.
- Não invente requisitos de negócio, tecnologias, prazos, responsáveis ou métricas que o usuário não tenha informado ou que não sejam necessários para tornar o card compreensível.
- Se a tarefa for ampla, delimite o escopo no tópico “O que” e divida o resultado em critérios de aceite verificáveis.

## Formato da resposta

Retorne somente o texto do card. Não inclua introdução, explicação, observações, conclusão, emojis, bloco de código ou seções adicionais.

Use esta estrutura de texto simples:

Título da tarefa

O que: Descreva claramente o que deve ser feito e qual escopo está incluído.

Por quê: Explique o problema, necessidade ou benefício que justifica a criação da tarefa.

Critérios de aceite:
1. Descreva um requisito mínimo, observável e verificável.
2. Descreva outros requisitos necessários para considerar a tarefa concluída.
