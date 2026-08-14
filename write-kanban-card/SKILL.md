---
name: write-kanban-card
description: Create simple, objective text for generic Kanban task cards, always including a title and the sections “O que”, “Por quê”, and “Critérios de aceite”. Use when the user asks to write, draft, structure, or turn a requirement, feature, fix, or improvement into a Kanban task card.
---

# Write Kanban Card

Transform the user's request into a single text ready to copy into a Kanban task card.

## Procedure

1. Identify the task, expected outcome, and context provided by the user.
2. If essential information is missing to describe the task, ask focused questions before drafting the card. Treat the task objective and its reason as essential.
3. Create a short, specific, action-oriented title that starts with a Portuguese action verb. Do not use “Card”, “Tarefa”, or placeholders in the title.
4. Draft objective, testable acceptance criteria without returning any card text yet.
5. Present one suggested criterion at a time through the native question-and-answer interaction. Offer the choices to approve it or request an adjustment, in PT-BR.
6. If the user requests an adjustment, ask for the desired change in free text, revise only the current criterion, and present it again for approval.
7. Move to the next criterion only after the current one is explicitly approved. Do not write the final card until every suggested criterion is approved.
8. After every criterion is approved, write the final card with only a title and the three required sections.

## Content Rules

- Always include the labels `O que:`, `Por quê:`, and `Critérios de aceite:` with this exact spelling.
- Write all generated card content in Brazilian Portuguese (PT-BR), unless the user explicitly requests another language.
- Start every title with a Portuguese action verb, such as “Implementar”, “Adicionar”, “Corrigir”, or “Atualizar”. Make the title reflect the task outcome rather than only its general topic.
- Write acceptance criteria as objective, testable completion conditions. Include every minimum criterion identified in the request.
- Never include acceptance criteria in the final card without the user's explicit approval. Preserve the approval order in the final card.
- Do not invent business requirements, technologies, deadlines, owners, or metrics that the user did not provide or that are not necessary to make the card understandable.
- If the task is broad, define its scope under “O que” and divide the outcome into verifiable acceptance criteria.

## Approval Flow

Before generating the card, use the native question-and-answer interaction when it is available. Present exactly one suggested acceptance criterion per interaction. Include the criterion in the question and offer these choices in PT-BR:

- `Aprovar critério`
- `Solicitar ajuste`

When the user chooses `Solicitar ajuste`, request the desired change in free text. Revise the current criterion and repeat its approval interaction. Do not show a draft or partial card during this flow.

If the native interaction is unavailable, ask for approval or a requested adjustment conversationally, one criterion at a time. Do not show a draft or partial card during this flow.

## Final Response Format

After every suggested criterion is approved, return only the final card text. Do not include an introduction, explanation, notes, conclusion, emojis, code fences, or additional sections.

Use this plain-text structure. Keep the resulting content in PT-BR:

Título da tarefa

O que: Descreva claramente o que deve ser feito e qual escopo está incluído.

Por quê: Explique o problema, necessidade ou benefício que justifica a criação da tarefa.

Critérios de aceite:
1. Descreva um requisito mínimo, observável e verificável.
2. Descreva outros requisitos necessários para considerar a tarefa concluída.
