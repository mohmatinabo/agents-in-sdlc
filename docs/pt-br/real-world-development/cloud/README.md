---
slug: pt-br/real-world-development/cloud
title: "Agente de nuvem do GitHub Copilot"
description: "Aprenda a usar o agente de nuvem do GitHub Copilot em um fluxo de trabalho assíncrono, da configuração do repositório à revisão e iteração em pull requests."
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui tarefas no GitHub e o agente de nuvem as executa em segundo plano — explora o repositório, faz alterações e abre um pull request — enquanto você pode se dedicar a outras atividades.

Nestes exercícios, você adicionará instruções personalizadas que o agente de nuvem seguirá, atribuirá uma issue do GitHub e deixará que ele implemente o trabalho. Você revisará e usará agentes personalizados para orientar a abordagem, monitorará e direcionará as sessões no painel de agentes e terminará revisando os pull requests e fazendo iterações sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e Codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione as instruções personalizadas que o agente de nuvem seguirá |
| [2. Agente de nuvem][ex2] | Agente assíncrono | Atribua issues ao agente de nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Monitore e oriente sessões de agentes |
| [5. Fazer iterações][ex5] | Revisão | Revise PRs, faça iterações no trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O agente de nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop — assim, você pode concluir todos os ambientes com esse plano.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O agente de nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — em todos, exceto no Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para a organização.

> [!NOTE]
> Este ambiente não aborda MCP. Para aprender a usar servidores MCP com o Copilot, consulte o [ambiente da CLI][cli-harness] ou o [ambiente do VS Code][vscode-harness].

## Começar

**[Comece pelos pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
