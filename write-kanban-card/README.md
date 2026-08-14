# Skill Write Kanban Card

A skill **Write Kanban Card** transforma requisitos, funcionalidades, correções e melhorias em textos objetivos, prontos para serem copiados para um card Kanban.

Além de organizar a descrição da tarefa, a skill conduz a aprovação dos critérios de aceite individualmente. O card final só é apresentado depois que todos os critérios sugeridos forem aprovados pelo usuário.

## Sumário

- [O que a skill faz](#o-que-a-skill-faz)
- [Quando usar](#quando-usar)
- [Como usar](#como-usar)
- [Como a skill cria um card](#como-a-skill-cria-um-card)
- [Fluxo de aprovação](#fluxo-de-aprovação)
- [Formato do card final](#formato-do-card-final)
- [Como escrever bons critérios de aceite](#como-escrever-bons-critérios-de-aceite)
- [Exemplo completo](#exemplo-completo)
- [Regras de conteúdo](#regras-de-conteúdo)
- [Boas práticas](#boas-práticas)
- [Limitações](#limitações)

## O que a skill faz

A skill pode ser utilizada para:

- transformar uma solicitação informal em um card Kanban estruturado;
- criar um título curto, específico e orientado à ação;
- descrever claramente o que precisa ser feito;
- registrar o problema, a necessidade ou o benefício que justifica a tarefa;
- sugerir critérios de aceite objetivos e verificáveis;
- solicitar a aprovação de cada critério antes de incluí-lo no card;
- ajustar um critério sem alterar os demais;
- produzir um texto final simples, sem comentários adicionais, pronto para copiar.

O resultado sempre contém um título e as seções `O que:`, `Por quê:` e `Critérios de aceite:`.

## Quando usar

Use a skill quando precisar criar um card Kanban para:

- implementar uma funcionalidade;
- corrigir um defeito;
- melhorar um comportamento existente;
- atualizar uma interface, integração ou documentação;
- executar uma atividade técnica ou operacional;
- transformar um requisito amplo em uma tarefa com resultado verificável;
- revisar e organizar uma descrição de tarefa antes de adicioná-la ao quadro.

A skill é adequada para cards genéricos e não depende de uma ferramenta específica de gestão. O texto pode ser copiado para o sistema Kanban utilizado pela equipe.

## Como usar

Ative a skill pelo nome e descreva a tarefa:

```text
Use $write-kanban-card para criar um card sobre a implementação de recuperação
de senha por e-mail. Precisamos permitir que usuários que esqueceram a senha
recuperem o acesso sem entrar em contato com o suporte.
```

Para que a skill consiga elaborar o card, informe pelo menos:

1. o objetivo da tarefa;
2. o motivo pelo qual ela precisa ser realizada.

Também podem ser fornecidos, quando relevantes:

- comportamento esperado;
- escopo incluído ou excluído;
- regras de negócio conhecidas;
- condições necessárias para considerar a tarefa concluída;
- restrições já definidas pelo projeto.

Se o objetivo ou a justificativa estiverem ausentes, a skill faz perguntas focadas antes de sugerir o card. Ela não inventa requisitos de negócio para preencher informações essenciais.

### Exemplos de solicitação

Criar um card para uma funcionalidade:

```text
Use $write-kanban-card para criar um card sobre a adição de filtros por status
na listagem de pedidos. O objetivo é ajudar o time de atendimento a localizar
pedidos pendentes com mais rapidez.
```

Criar um card para uma correção:

```text
Use $write-kanban-card para escrever uma tarefa de correção. O total do carrinho
não é recalculado quando um cupom é removido, o que pode exibir um valor incorreto
antes da finalização da compra.
```

Criar um card para uma melhoria:

```text
Use $write-kanban-card para estruturar uma melhoria na busca de clientes.
Ela deve aceitar consultas por nome e e-mail para reduzir o tempo gasto pelo suporte.
```

## Como a skill cria um card

O fluxo seguido pela skill é:

1. identificar a tarefa, o resultado esperado e o contexto fornecido;
2. confirmar que o objetivo e a justificativa estão claros;
3. criar um título curto que comece com um verbo de ação em português;
4. preparar critérios de aceite objetivos e testáveis, sem apresentar ainda o card;
5. sugerir exatamente um critério por vez para aprovação;
6. ajustar o critério atual quando solicitado e apresentá-lo novamente;
7. avançar somente depois da aprovação explícita do critério atual;
8. apresentar o card final apenas quando todos os critérios estiverem aprovados.

Esse processo evita que condições não confirmadas sejam incluídas silenciosamente no card.

## Fluxo de aprovação

Cada critério sugerido é apresentado individualmente com duas opções:

- `Aprovar critério`;
- `Solicitar ajuste`.

Ao escolher `Aprovar critério`, o usuário confirma o critério e a skill apresenta o próximo, quando houver.

Ao escolher `Solicitar ajuste`, a skill pede a alteração desejada em texto livre, modifica somente o critério atual e o apresenta novamente para aprovação. Os critérios já aprovados permanecem inalterados.

O card parcial não é exibido durante esse processo. A ordem de aprovação dos critérios é preservada no resultado final.

Quando a interação nativa de perguntas e respostas não estiver disponível, a skill conduz a mesma aprovação por meio da conversa, sempre com um critério por vez.

## Formato do card final

Depois da aprovação de todos os critérios, a skill retorna somente o texto do card, seguindo esta estrutura:

```text
Título da tarefa

O que: Descrição clara da tarefa e do escopo incluído.

Por quê: Problema, necessidade ou benefício que justifica a tarefa.

Critérios de aceite:
1. Condição mínima, observável e verificável.
2. Outra condição necessária para considerar a tarefa concluída.
```

O resultado não inclui introdução, explicações, notas, conclusão, emojis, blocos adicionais ou marcações que não façam parte do card.

### Título

O título começa com um verbo de ação em português, como:

- `Adicionar`;
- `Atualizar`;
- `Corrigir`;
- `Implementar`;
- `Permitir`.

Ele descreve o resultado esperado e não utiliza palavras genéricas como “Card”, “Tarefa” ou placeholders.

### O que

A seção `O que:` descreve a mudança esperada e delimita o escopo quando a solicitação é ampla. O texto deve permitir que outra pessoa compreenda a atividade sem depender do histórico completo da conversa.

### Por quê

A seção `Por quê:` apresenta a necessidade, o problema ou o benefício que motivou o card. Ela explica o valor da tarefa sem adicionar justificativas que não tenham sido fornecidas.

### Critérios de aceite

A seção `Critérios de aceite:` reúne, na ordem de aprovação, as condições necessárias para considerar a tarefa concluída.

## Como escrever bons critérios de aceite

Um critério adequado descreve um resultado que pode ser observado ou verificado. Ele deve ser específico o suficiente para indicar conclusão, sem inventar decisões técnicas ou de negócio.

Exemplos objetivos:

- `O usuário consegue solicitar a recuperação informando um e-mail cadastrado.`
- `O total do carrinho é recalculado após a remoção de um cupom.`
- `A listagem permite filtrar pedidos pelo status selecionado.`

Exemplos vagos que devem ser evitados:

- `A funcionalidade deve funcionar corretamente.`
- `Melhorar a experiência do usuário.`
- `Garantir qualidade.`

Quando a tarefa é ampla, a skill divide o resultado em critérios independentes e verificáveis, mantendo apenas as condições necessárias para o escopo informado.

## Exemplo completo

Solicitação:

```text
Use $write-kanban-card para criar um card de recuperação de senha por e-mail.
Usuários que esqueceram a senha precisam recuperar o acesso sem acionar o suporte.
```

Durante a interação, a skill apresenta um critério por vez. Por exemplo:

```text
O usuário consegue solicitar a recuperação informando um e-mail cadastrado.

Opções: Aprovar critério | Solicitar ajuste
```

Somente depois que todos os critérios sugeridos forem aprovados, a skill produz um resultado como:

```text
Implementar recuperação de senha por e-mail

O que: Permitir que usuários solicitem a recuperação de acesso por meio do e-mail cadastrado.

Por quê: Possibilitar que usuários que esqueceram a senha recuperem o acesso sem entrar em contato com o suporte.

Critérios de aceite:
1. O usuário consegue solicitar a recuperação informando um e-mail cadastrado.
2. O usuário recebe as instruções necessárias para definir uma nova senha.
3. O usuário consegue acessar a conta utilizando a nova senha.
```

Os critérios acima são ilustrativos. Em uma utilização real, cada um deve ser explicitamente aprovado antes de aparecer no card final.

## Regras de conteúdo

- Usar exatamente os rótulos `O que:`, `Por quê:` e `Critérios de aceite:`.
- Escrever o card em português brasileiro, salvo quando outro idioma for solicitado explicitamente.
- Iniciar o título com um verbo de ação em português.
- Manter o título específico e orientado ao resultado.
- Escrever critérios observáveis e testáveis.
- Incluir somente critérios aprovados pelo usuário.
- Preservar a ordem em que os critérios foram aprovados.
- Não inventar requisitos, tecnologias, prazos, responsáveis ou métricas.
- Delimitar o escopo em `O que:` quando a tarefa for ampla.

## Boas práticas

- Informe o objetivo e a justificativa já na primeira solicitação.
- Descreva o resultado esperado em vez de apenas indicar o assunto da tarefa.
- Forneça regras de negócio relevantes quando elas já estiverem definidas.
- Peça ajustes sempre que um critério estiver ambíguo ou incluir algo fora do escopo.
- Mantenha cada critério focado em uma condição verificável.
- Revise o card final antes de copiá-lo para o quadro Kanban.

## Limitações

- A qualidade do card depende do contexto fornecido pelo usuário.
- A skill não define prioridade, prazo, responsável, estimativa ou métricas sem que essas informações sejam fornecidas.
- A skill não cria automaticamente um item em uma ferramenta de gestão; ela produz o texto do card.
- O formato é intencionalmente simples e contém somente as três seções obrigatórias.
- O fluxo de aprovação pode exigir várias interações quando houver muitos critérios.

As instruções operacionais que definem o comportamento da skill estão em [`SKILL.md`](./SKILL.md).
