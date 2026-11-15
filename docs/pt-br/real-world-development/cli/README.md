---
slug: pt-br/real-world-development/cli
title: "CLI do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

O **[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** coloca o GitHub Copilot no terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela interface de linha de comando (CLI), para que você mantenha o foco sem trocar para um editor gráfico.

Ao longo destas lições, você instalará e autenticará o Copilot CLI, depois fornecerá contexto do projeto com instruções personalizadas antes de usar os modos Plan e Autopilot para desenvolver um recurso. Você conectará o servidor do Model Context Protocol (MCP) do Playwright para testar esse recurso em um navegador real e, em seguida, ampliará o Copilot com skills de agente reutilizáveis e agentes personalizados. Por fim, você usará o Agent Merge para abrir e mesclar um pull request, explorará comandos de barra para gerenciar contexto, modelos e compartilhamento e concluirá com uma revisão do que criou. Você também pode seguir uma [série opcional de três módulos com GitHub Copilot CLI e Foundry][foundry] para preparar um modelo, criar e implantar um agente hospedado e integrá-lo ao site.

## Lições

| Lição | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu Codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI |
| [2. Adicionar avaliações por estrelas][ex2] | Primeira alteração | Adicione uma avaliação por estrelas aos cards de jogos |
| [3. Modos de agente][ex3] | Modos | Use Plan e Autopilot para implementar um recurso |
| [4. Instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções do projeto |
| [5. Skills de agente][ex5] | Skills | Personalize uma skill reutilizável de verificação de qualidade |
| [6. Validar com o Playwright MCP][ex6] | Ferramentas externas | Teste o recurso em um navegador com o servidor MCP do Playwright |
| [7. Agente de QA][ex7] | Agentes personalizados | Crie e use um agente de QA especializado |
| [8. Criar e mesclar o PR do recurso][ex8] | Pull requests | Revise a filtragem e use o Agent Merge |
| [9. Recursos avançados da CLI][ex9] | Comandos de barra | Explore o contexto, os modelos, o compartilhamento e a delegação opcional para a nuvem |
| [10. Encerramento][ex10] | Revisão | Recapitule o fluxo de trabalho e os próximos passos |
| [Opcional: Incorpore o Foundry][foundry] | Agentes hospedados | Prepare um modelo, crie e implante um Backer Concierge baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Familiaridade básica com operações de terminal e linha de comando
- [ ] O Git instalado e configurado

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop. Portanto, você pode concluir todos os percursos com esse plano.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, verifique se o administrador habilitou o Copilot CLI para uso.

## Comece

[**Comece pela Lição 0: Pré-requisitos →**][ex0]

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
[callout-student-plan-education]: https://github.com/education/students
