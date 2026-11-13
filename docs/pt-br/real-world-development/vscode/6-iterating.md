---
title: "Exercício 6 - Fazer iterações no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário. Usou o modo agente para adicionar filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente esse trabalho em um navegador e, depois, um agente personalizado para implementar recursos de acessibilidade — controles para alternar os modos de alto contraste e claro — e orientou a sessão em andamento para ampliar o trabalho. Agora é hora de publicar esse trabalho local e revisá-lo da mesma forma que sua equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a chegada da IA generativa. Você ainda precisa de um ciclo real de revisão para tudo o que o Copilot produz. Com isso em mente, vamos enviar as alterações de acessibilidade do Codespace ao repositório remoto, abrir um pull request e examinar o diff antes de envolver o restante da equipe.
## Publicar os recursos de acessibilidade

Os controles dos modos de alto contraste e claro que você implementou com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no Codespace como alterações registradas em commits. Vamos enviá-las para uma branch e abrir um pull request para que o restante da equipe possa revisar.

1. Volte ao Codespace.
2. Abra a visualização **Source Control** no VS Code.
3. Confirme que as alterações de acessibilidade estão registradas em commits. Se houver alterações do Exercício 5 ainda não registradas, prepare-as e faça commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** (ou use o menu **...** → **Push**).
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite a solicitação ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro (por exemplo, `Add high-contrast and light-mode toggles`) e uma descrição curta que resuma o que foi feito e por quê.
7. Selecione **Create pull request**.
8. Depois que o PR estiver aberto, selecione a guia **Files changed** para revisar o trabalho de ponta a ponta. Preste atenção especial a:
   - componentes da interface para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências do usuário.
   - alterações de CSS ou de estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que controla a alternância entre modos.

9. Volte à guia **Conversation**.
10. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos fluxos de trabalho. Se tudo correr bem, eles deverão passar.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Marque `@copilot` em um comentário no PR com uma solicitação, como "revise este PR em busca de outros problemas relacionados às WCAG" ou "sugira melhorias na navegação por teclado". O Copilot iniciará uma nova sessão para responder ao comentário.

## Exercício opcional - continuar explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade, e a única forma de desenvolvê-la é praticar. Algumas ideias para próximas sessões que você pode executar no VS Code:

- Adicionar um formulário para demonstrar interesse no jogo na página de detalhes.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados em `src/lib/`.
- Ampliar o escopo do agente de acessibilidade — por exemplo, auditar a ordem de foco do teclado em todo o site.

## Resumo

Parabéns — você concluiu o ambiente do VS Code! Neste laboratório, você:

- **Usou o MCP do Playwright para testar manualmente seu recurso.** Adicionou o servidor MCP do Playwright e deixou o Copilot controlar um navegador para verificar a filtragem antes de abrir um pull request.
- **Orientou o modo agente durante alterações coordenadas em toda a pilha.** Adicionou um recurso de filtragem que envolveu o cliente, o servidor e os testes em uma única sessão.
- **Usou um agente personalizado.** Selecionou o agente personalizado de acessibilidade no seletor de agentes e acompanhou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Revisou as alterações propostas diretamente no editor, aceitou o que queria e ampliou a sessão com um pedido complementar para o modo claro.
- **Concluiu o ciclo com um pull request.** Publicou o trabalho local e o revisou de ponta a ponta da mesma forma que sua equipe faria.

## Revisão e próximos passos

Este é o fim do ambiente obrigatório do VS Code. Você pode parar aqui com o workshop concluído.

Se quiser ampliar sua perspectiva sobre os recursos de agente do Copilot, os outros ambientes abordam cenários relacionados em interfaces diferentes:

- 💻 **[Ambiente da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente de QA personalizado, MCP do Playwright, Agent Merge e transferências com reconhecimento de contexto.
- ☁️ **[Ambiente do agente de nuvem](../../cloud/)** — concentre-se em atribuir issues ao agente de nuvem, monitorar sessões pela página de agentes e fazer iterações assíncronas em pull requests.

Você também pode continuar desenvolvendo o que começou aqui. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que você pode adaptar aos seus projetos.

Como extensão opcional, [Opcional: Incorporar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
