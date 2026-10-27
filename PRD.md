# PRD: Caça-palavras Intervalo

## 1. Visão do produto

O Caça-palavras Intervalo é um jogo leve de navegador para uma pausa curta. A pessoa escolhe um tema e uma dificuldade, encontra palavras em uma grade e acompanha o tempo da partida. O jogo é uma página estática, em português do Brasil, que funciona sem conta ou serviço de backend.

## 2. Objetivos

- Oferecer partidas rápidas e fáceis de iniciar.
- Permitir variar o conteúdo por tema e o desafio por dificuldade.
- Dar retorno claro sobre seleção, acertos, progresso e conclusão.
- Funcionar em telas grandes e estreitas e respeitar preferências de movimento reduzido.
- Salvar localmente o melhor tempo para cada tema e dificuldade.

## 3. Público e contexto de uso

O produto é destinado a pessoas que querem jogar um caça-palavras casual no navegador, em uma pausa ou em um dispositivo compartilhado. Não exige cadastro, instalação ou conhecimento prévio além de selecionar letras na grade.

## 4. Escopo funcional

### RF-01: Iniciar e reiniciar uma partida

Ao abrir a página, o jogo deve gerar uma partida. O comando de nova partida deve gerar outra grade para as opções selecionadas, zerar o progresso e iniciar novamente o cronômetro.

**Critérios de aceite**
- A grade preenchida e a lista de palavras aparecem ao carregar a página.
- Nova partida apresenta uma grade e uma lista atualizadas, sem palavras marcadas como encontradas.
- O cronômetro começa em `00:00` para a nova partida.

### RF-02: Escolher tema

A pessoa pode escolher entre Natureza, Comida, Espaço e Animais. A mudança de tema inicia uma nova partida e atualiza o recorde exibido para a combinação selecionada.

### RF-03: Escolher dificuldade

A pessoa pode escolher entre Fácil, Médio e Difícil. As configurações atuais são:

| Dificuldade | Grade | Palavras |
| --- | ---: | ---: |
| Fácil | 10 × 10 | 6 |
| Médio | 12 × 12 | 8 |
| Difícil | 14 × 14 | 10 |

A mudança de dificuldade inicia uma nova partida e atualiza o recorde correspondente.

### RF-04: Encontrar palavras

As palavras são formadas em linha reta na horizontal, vertical ou diagonal, em qualquer sentido. Para selecionar uma palavra, a pessoa toca ou clica na primeira e na última letra. A seleção invertida também deve ser reconhecida.

**Critérios de aceite**
- O primeiro toque seleciona a célula inicial e solicita a célula final.
- Uma seleção reta válida que corresponda a uma palavra ainda não encontrada marca a palavra e suas células.
- Uma seleção fora das direções permitidas ou que não corresponda a uma palavra não aumenta o progresso e apresenta uma mensagem de orientação.
- Cada palavra encontrada é marcada na lista e não pode ser contada novamente.

### RF-05: Acompanhar progresso

O jogo deve exibir a quantidade de palavras encontradas em relação ao total, uma indicação visual de progresso e as palavras restantes.

### RF-06: Usar dica

A dica escolhe uma palavra ainda não encontrada, destaca sua primeira célula e informa a linha e a coluna em que ela começa. Se não houver palavras restantes, a ação não altera a partida.

### RF-07: Cronometrar e concluir

O cronômetro mede o tempo decorrido desde o início da partida. Ao encontrar todas as palavras, o jogo interrompe a contagem, apresenta uma mensagem de conclusão e exibe o tempo final.

### RF-08: Salvar melhor tempo

O melhor tempo é salvo no armazenamento local do navegador, separado por tema e dificuldade. Ao concluir uma partida, um tempo menor substitui o recorde anterior; um tempo maior não o substitui. Na ausência de recorde, a interface exibe `--:--`.

## 5. Requisitos de experiência e qualidade

- **Idioma:** textos, controles e mensagens devem permanecer em português do Brasil. As palavras da grade são armazenadas sem acentos.
- **Responsividade:** a grade, os seletores, os controles e a lista devem permanecer utilizáveis em telas estreitas.
- **Acessibilidade:** usar controles semânticos, foco visível, nomes acessíveis para controles e células, estados de progresso anunciáveis e mensagens dinâmicas em região `aria-live`.
- **Movimento:** respeitar `prefers-reduced-motion` e reduzir ou remover animações quando essa preferência estiver ativa.
- **Privacidade e disponibilidade:** não exigir conta, backend ou envio de dados. O recorde permanece no armazenamento local do navegador.
- **Compatibilidade de execução:** manter o jogo abrível diretamente como arquivo HTML, sem etapa de build ou dependências obrigatórias.

## 6. Fora de escopo

- Contas, perfis, sincronização ou ranking online.
- Modos multiplayer, competição ou compartilhamento de resultados.
- Instalação como aplicativo ou dependência de servidor para iniciar o jogo.
- Novas mecânicas não descritas neste documento, como pontuação, penalidade por dica ou limite de tempo.

## 7. Validação

Para mudanças no jogo, abrir `index.html` em um navegador e percorrer os fluxos de início, troca de tema e dificuldade, seleção correta e incorreta, dica e conclusão. Conferir também o layout em tela estreita e a persistência do recorde após recarregar a página. O repositório não possui suíte automatizada nem comando de build configurado.