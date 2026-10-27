# Instrucoes para agentes

## Projeto
- Este repositório contém um jogo de caça-palavras estático, implementado em `index.html` com HTML, CSS e JavaScript no mesmo arquivo.
- `README.md` contém apenas o título do projeto. Não há framework, gerenciador de dependências, etapa de build ou suíte de testes configurados.
- A interface e as mensagens são em português do Brasil.

## Diretrizes de implementação
- Mantenha a solução simples e compatível com a abertura direta de `index.html` no navegador. Não introduza dependências, ferramentas de build ou arquivos adicionais sem necessidade para a tarefa.
- Preserve as regras existentes: temas e dificuldades configuráveis, palavras nas oito direções, seleção por primeira e última letra, dicas, cronômetro e melhor tempo salvo por tema e dificuldade.
- Ao alterar a interface, siga os estilos e componentes já definidos no próprio arquivo; preserve o layout responsivo e o suporte a `prefers-reduced-motion`.
- Mantenha a acessibilidade existente: controles semânticos, estados de foco visíveis, rótulos ARIA e mensagens anunciadas por `aria-live`.
- Preserve o idioma pt-BR em textos visíveis e mensagens. As palavras da grade são armazenadas sem acentos.
- Evite alterar comportamentos não relacionados ao pedido.

## Verificação
- Para mudanças na interface ou no jogo, abra `index.html` em um navegador e verifique o fluxo afetado, incluindo a apresentação em tela estreita quando pertinente.
- Não há comandos de build ou testes automatizados configurados. Não invente uma etapa de validação como se fosse parte do projeto.