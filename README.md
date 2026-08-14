# basic_skills

Repositório de skills básicas e reutilizáveis para os projetos da organização.

As skills ajudam agentes baseados em LLMs a executar tarefas como integrações com plataformas, criação de código, documentação, diagramas e automações.

## Estrutura

Cada skill deve ficar em um diretório próprio, identificado por um nome curto, descritivo e em letras minúsculas, com palavras separadas por hífen.

O diretório da skill deve conter obrigatoriamente um arquivo `SKILL.md`, com seu objetivo, pré-requisitos, instruções de uso, limites e formas de validação. Arquivos complementares, como scripts, templates, exemplos ou referências, devem permanecer dentro do mesmo diretório da skill, organizados em subdiretórios quando necessário.

Skills relacionadas devem ser agrupadas em diretórios de categoria. Evite duplicar instruções e mantenha cada skill independente, para que possa ser utilizada em diferentes projetos sem depender de arquivos externos ao seu diretório.

## Conteúdo das skills

Cada skill deve resolver uma necessidade específica e recorrente. O `SKILL.md` deve explicar de forma objetiva quando utilizá-la, quais acessos ou ferramentas são necessários, quais passos devem ser executados e como verificar o resultado.

As instruções devem ser completas o suficiente para que a skill seja usada por diferentes projetos, sem conhecimento adicional do autor. Quando houver comportamentos relevantes, inclua exemplos, limites conhecidos e orientações para falhas.

## Segurança e revisão

Não inclua credenciais, tokens, dados pessoais ou informações confidenciais. Prefira variáveis de ambiente, mecanismos de segredo aprovados e dados fictícios nos exemplos.

Toda nova skill ou alteração deve ser enviada para revisão por pull request. Antes disso, verifique se já existe uma skill equivalente e valide que as instruções estão claras, atualizadas e funcionais.
