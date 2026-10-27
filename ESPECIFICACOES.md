# Especificações: Caça-palavras Intervalo

Este documento detalha os contratos de conteúdo, interação e execução do jogo. O PRD descreve objetivos e requisitos do produto; este arquivo registra os parâmetros e comportamentos concretos da implementação atual.

## 1. Execução

- Aplicação estática em `index.html`, com HTML, CSS e JavaScript no mesmo arquivo.
- Deve funcionar ao abrir o arquivo diretamente em um navegador moderno.
- Não requer backend, conta, instalação, etapa de build ou dependências JavaScript.
- As fontes DM Sans e Fraunces são carregadas do Google Fonts; devem existir fontes de fallback.
- A interface, mensagens e controles visíveis usam português do Brasil.

## 2. Conteúdo e configuração

### Temas

Cada tema possui um banco de dez palavras em letras maiúsculas sem acentos:

| Tema | Palavras |
| --- | --- |
| Natureza | FLORESTA, CACHOEIRA, MONTANHA, BORBOLETA, GIRASSOL, PASSARO, ORQUIDEA, RIACHO, VENTO, FOLHA |
| Comida | ABACAXI, CHOCOLATE, MORANGO, PANQUECA, LARANJA, BISCOITO, PIPOCA, MACARRAO, QUEIJO, MELANCIA |
| Espaço | PLANETA, GALAXIA, ESTRELA, SATELITE, COMETA, COSMOS, ORBITA, LUNAR, FOGUETE, NEBULOSA |
| Animais | GOLFINHO, TARTARUGA, ELEFANTE, BORBOLETA, CANGURU, JACARE, CORUJA, PANTERA, COELHO, GIRAFA |

### Dificuldades

| Valor interno | Rótulo | Dimensão da grade | Quantidade de palavras |
| --- | --- | ---: | ---: |
| `facil` | Fácil | 10 × 10 | 6 |
| `medio` | Médio | 12 × 12 | 8 |
| `dificil` | Difícil | 14 × 14 | 10 |

As palavras escolhidas para uma partida são ordenadas da maior para a menor antes da tentativa de posicionamento.

## 3. Geração da grade

- A grade é quadrada e as células vazias são preenchidas com letras aleatórias de `A` a `Z`.
- Cada palavra pode ser posicionada em uma das oito direções: horizontal, vertical ou diagonal, em qualquer sentido.
- O posicionamento deve permanecer dentro dos limites da grade. Uma célula ocupada pode ser reutilizada quando contém a mesma letra; letras diferentes não podem se sobrepor.
- A implementação atual tenta até 400 posições aleatórias por palavra. Somente palavras posicionadas são incluídas na lista ativa da partida.
- A composição das palavras e letras de preenchimento é aleatória a cada nova partida.

## 4. Estado e ciclo da partida

O estado da partida contém:

- dimensão da grade e matriz de letras;
- palavras ativas e coordenadas de cada palavra posicionada;
- conjunto de palavras encontradas;
- coordenada inicial da seleção em andamento, se houver;
- instante de início, tempo decorrido e indicador de partida concluída.

Ao carregar a página, iniciar uma nova partida ou mudar tema/dificuldade, o jogo cria uma grade, limpa as palavras encontradas e inicia o cronômetro. Tema e dificuldade permanecem nos seletores durante a partida. Ao concluir, novas seleções de células não alteram o resultado; iniciar outra partida libera a interação novamente.

## 5. Interações

### Seleção de palavras

1. O primeiro clique ou toque em uma célula marca a origem e solicita a letra final.
2. O segundo clique define o destino. A seleção é válida somente quando origem e destino formam uma linha horizontal, vertical ou diagonal reta.
3. O texto percorrido é comparado com as palavras ainda não encontradas, tanto na ordem direta quanto na ordem inversa.
4. Um acerto marca as células e a palavra correspondente, atualiza o progresso e verifica se a partida terminou.
5. Uma sequência inválida não altera o conjunto de palavras encontradas. As mensagens de estado explicam a próxima ação ou o motivo da rejeição.

### Dica

- Escolhe aleatoriamente uma palavra ainda não encontrada.
- Destaca a primeira célula dessa palavra e anuncia sua linha e coluna, ambas numeradas a partir de 1.
- Não há cobrança ou pontuação associada à dica.
- Se todas as palavras já foram encontradas, a ação não faz nada.

### Progresso e conclusão

- O contador apresenta `encontradas / total`; a barra de progresso expõe os valores acessíveis mínimo, máximo e atual.
- Ao encontrar a última palavra, a partida é marcada como concluída, o tempo final é apresentado e uma mensagem de sucesso é exibida.

## 6. Cronômetro e recordes

- O tempo é calculado em segundos desde o início da partida e atualizado visualmente a cada segundo.
- A apresentação usa `MM:SS`, com no mínimo dois dígitos para minutos e segundos.
- O recorde é independente para cada combinação de tema e dificuldade.
- A chave de armazenamento tem o formato `intervalo-best-{tema}-{dificuldade}`; o valor armazenado é o tempo em segundos como texto.
- Um recorde só é gravado ao concluir a partida e somente quando não existe recorde anterior ou o tempo atual é menor.
- Os dados ficam no `localStorage` do navegador e não são sincronizados entre dispositivos ou navegadores.

## 7. Interface e acessibilidade

- A página contém cabeçalho, título, seletor de tema, seletor de dificuldade, ação de dica, ação de nova partida, estado e cronômetro, grade, mensagem dinâmica, progresso, lista de palavras e recorde.
- Os seletores e botões devem ser operáveis por teclado e apresentar foco visível. As células da grade têm nome acessível com letra, linha e coluna.
- Mensagens da partida são anunciadas por uma região `aria-live`; a lista e o progresso devem continuar compreensíveis sem depender apenas de cor.
- A grade usa células quadradas e adapta seu tamanho ao espaço disponível.
- Em larguras de até 760 px, o painel de palavras fica abaixo da área de jogo e os botões de ação tornam-se compactos. Até 370 px, os controles e a lista ajustam sua disposição para caber na tela.
- A regra `prefers-reduced-motion: reduce` reduz animações e transições.

## 8. Persistência, privacidade e limites

- O único dado persistido pelo jogo é o melhor tempo local por tema e dificuldade.
- Não há coleta ou envio de dados, autenticação, perfil, classificação online, modo multiplayer ou sincronização.
- Limpar os dados do site no navegador remove os recordes salvos.

## 9. Verificação manual

Ao validar mudanças, conferir no navegador:

- carregamento inicial e nova partida;
- alteração de tema e dificuldade, incluindo tamanho da grade e quantidade de palavras;
- acertos em direções direta e inversa, seleção inválida e atualização do progresso;
- dica com coordenadas coerentes;
- conclusão, parada do cronômetro e atualização do recorde;
- persistência após recarregar e isolamento do recorde por tema/dificuldade;
- uso em tela estreita, foco por teclado e preferência por movimento reduzido.

Não há suíte automatizada nem comando de build configurado no repositório.