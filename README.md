https://github.com/ribollequis87/aps3_cdados.git# APS 3 - Otimização de Portifolio de Investimentos

## Disclaimer

Nesta atividade, vamos usar o que aprendemos aqui para otimizar uma carteira de investimentos. Um disclaimer importante é que você provavelmente não deveria usar somente isso para compor sua própria carteira de investimentos. O motivo de usarmos este procedimento específico é que ele cabe bem no conteúdo que temos aprendido e, embora seja uma estratégia válida e importante, ela não observa vários fatores que também são relevantes na escolha de um portifolio. Se você quer fazer um portifolio de investimentos, estude mais sobre o assunto, incluindo outros critérios para a escolha dos componentes do portifolio. Se, mesmo assim, você quiser testar o seu portifolio em ação, use um [simulador de bolsas de valores](https://www.google.com/search?q=simulador+de+bolsa+de+valores&rlz=1C1DVJR_enCA884BR885&sxsrf=AB5stBhxFF_UxynuTjUhCA8kLrpPCRFJoA%3A1691500926024&ei=fkHSZMuOAdG95OUPn_ed-AI&gs_ssp=eJzj4tVP1zc0TDczTavKzSgwYPSSLc7MLc1JTMkvUkhJVUjKzylOBDHKEnPyi1KLAV2CD70&oq=simulador+de+bolsa+d&gs_lp=Egxnd3Mtd2l6LXNlcnAiFHNpbXVsYWRvciBkZSBib2xzYSBkKgIIADIIEC4YgAQYywEyCBAAGIAEGMsBMggQABiABBjLATIFEAAYgAQyBRAAGIAEMgUQABiABDIGEAAYFhgeMgYQABgWGB4yBhAAGBYYHjIGEAAYFhgeMhcQLhiABBjLARiXBRjcBBjeBBjgBNgBA0iHH1CpBliGF3ADeAGQAQCYAaQBoAHUE6oBBDAuMjC4AQPIAQD4AQHCAgoQABhHGNYEGLADwgIKEAAYigUYsAMYQ8ICDhAAGOQCGNYEGLAD2AEBwgIWEC4YigUYxwEY0QMYyAMYsAMYQ9gBAsICBxAjGIoFGCfCAgQQIxgnwgIHEAAYigUYQ8ICDRAuGIoFGMcBGNEDGEPCAgcQLhiKBRhD4gMEGAAgQYgGAZAGEroGBggBEAEYCboGBggCEAEYCLoGBggDEAEYFA&sclient=gws-wiz-serp).


## Introdução

Vários fundos de investimentos usam ferramentas computacionais para definir seus portifolios. Uma ferramenta muito conhecida para isso é o Índice Sharpee. Esse índice foi [proposto nos anos 60](https://www.jstor.org/stable/2351741) e parte das seguintes ideias:

## Retorno relativo

Um portifolio tem um retorno relativo igual a $R_t =(\text{preço}_t - \text{preço}_{t-1})/\text{preço}_{t-1}$. Por simplicidade, vamos dizer que $t$ é uma medida de tempo em dias. Então, se uma ação vale 10 reais no dia 0 e 11 reais no dia 1, o retorno relativo no dia 1 foi $R_1 = (11-10)/10 = 1/10 = 0.1 = 10\%$.

Dicas: 

1. Talvez seja preciso usar o método `.shift()` para deslocar a série de preços das ações.
1. Se estiver usando os dados do Yahoo Finance, use a coluna `Adj. Close` como referência para calcular o retorno.

## Aumentar ganhos, diminuir riscos

O índice sharpee é a divisão da esperança do retorno relativo pelo desvio padrão desse mesmo retorno, isto é:

$$
S = \frac{E[R-R_f]}{\sqrt{\text{Var}(R)}},
$$

onde $R_f$ é o retorno (diário) de um investimento sem risco (exemplo: tesouro SELIC, ou alguma estratégia que já esteja sendo usada anteriormente. Para nossos estudos, vamos assumir que não há investimento sem risco anterior, isto é, vamos usar $R_f=0$).

O índice aumenta quando a esperança do retorno aumenta ou quando seu desvio padrão diminui.

## Aumentando a esperança

Podemos calcular a esperança de ganhos de uma ação qualquer calculando a média de seus retornos ao longo de algum período de tempo. Também, se tivermos mais de uma ação, podemos usar as propriedades:

* $E(x+y) = E(x) + E(y)$
* $E(ax) = aE(x)$

para encontrar operações que aumentam a esperança de nosso portifólio. 

Por exemplo, se temos uma ação em que $E[R_1]=20$ e outra em que $E[R_2]=10$, ao somá-las (isto é, ao ter as duas ações em igual quantidade no portifólio) temos $E[R_1+R_2]=30$.

## Diminuindo o desvio padrão

Podemos calcular a variância dos retornos de uma ação também usando dados de algum período de tempo. Porém, ao combinarmos ações (isto é, ao ter mais de uma ação em nosso portifólio), temos as situações:

* $\text{Var}(x) = E(x^2) - (E(x))^2$
* $\text{Var}(ax) = a^2 \text{Var}(x)$
* $\text{Var}(x+y) = \text{Var}(x) + \text{Var}(y) + 2\text{Cov}(x,y)$

onde $\text{Cov}(x,y)$ é a covariância entre $x$ e $y$, que é uma medida análoga à correlação, exceto por ser multiplicada pelos desvios padrões de ambas as medidas.

Lembra-se do caso dos seguros, em que o desvio padrão cresce muito mais devagar que a média dos retornos quando aumentamos o número de clientes? Algo parecido acontece com ações. Ao combinarmos em nosso portifólio ações cujos retornos são descorrelacionados, o desvio padrão tende a crescer mais devagar que a média.

## Enunciado da atividade

Nesta atividade, você vai implementar e executar um algoritmo para encontrar um portifolio de ações baseado no índice Sharpee. Para isso, use dados de 2022 disponibilizados pela B3, ou pela biblioteca do Yahoo Finance.

1. Escolha pelo menos 20 ações candidatas da bolsa brasileira (B3). Para isso, use o critério que preferir.
1. Inicie com uma ação à sua escolha e coloque em seu portifolio
1. Calcule o índice Sharpee de seu portifolio
1. Calcule a correlação entre os retornos de seu portifolio e os retornos das ações candidatas restantes
1. Adicione em seu portifolio a ação cujos retornos são menos correlacionados a ele. Assuma que você tem um valor fixo para cada elemento do portifolio (por exemplo: tem sempre R$1000,00 investido em cada ação).
1. Volte ao passo 3 até encontrar 4 ações para compor seu portifolio.
1. Observe as ações que você escolheu. Em que setores as empresas operam?
1. Mostre numa figura como o Índice Sharpee se comporta a cada adição de uma nova ação.
1. Escreva um pequeno texto (até 500 palavras, mas pode ser menos) explicando porque o seu portifolio faz sentido. Para isso, use o que aprendemos sobre estatística, e também o que você souber sobre economia e investimentos. Se necessário, cite fontes.

# Entregas esperadas

Você deve entregar, via Blackboard, um arquivo PDF (uma entrega por grupo!) contendo:

1. Nome do grupo
1. Título do trabalho, que deve ser algo como "Otimizando portifólio de ações com índice Sharpee" (parafraseie isso!)
1. A figura gerada no ítem 8.
1. O texto gerado no ítem 9.
1. Uma auto-avaliação da figura e do texto, justificando as notas que foram auto-atribuídas.

# Rubricas e avaliação

Cada um dos ítens (exceto a auto-avaliação) será avaliado da seguinte forma:

| Rubrica | Descrição | Exemplo |
| --- | --- | --- | 
| F | Não entregue, entregue fora do prazo, entregue fora do escopo ("fiz outra coisa") | Não entregar |
| E | Entregue no prazo e dentro do escopo, mas a entrega tem um ou mais ítens faltando ou só é compreensível se for acompanhado deste enunciado (por exemplo, ao dizer "resposta ao ítem 2" ao invés de ter um título explicativo) ou está ilegível (exemplo: textos sem sentido ou figuras muito grandes / muito pequenas ou em resolução baixa) | Colocar no título de uma figura: "ítem 1a"
| D | A entrega tem falhas graves de coesão ou coerência que impedem sua compreensão, ou faltam elementos essenciais (título, legenda, rótulos nos eixos), ou o texto deixa de indicar o que são ideias originais e o que são ideias retiradas de outras fontes, ou as ideias/dados são mostrados de forma embaralhada, sem guiar o leitor a um fluxo de pensamento. | Em um texto: "Existe impacto da renda na educação. Isso porque a renda pode estar ligada a condições de estudo. O estudo é essencial para formar seres humanos"
| C | A entrega não tem falhas graves. Todas as fontes de ideias são indicadas. A entrega traz ao menos um elemento errado ou ao menos um elemento supérfluo (que não contribui para a mensagem passada). | Em um texto: "Essa é uma fonte de 100W (Watts, unidade batizada em homenagem a James Watt, inventor da máquina a vapor")
| B | A entrega tem todos os elementos necessários para passar a mensagem, a mensagem é clara, e não há elementos supérfluos. | Título da figura: "Média de desempenho no ENEM 2022 por disciplina por faixa de renda", com rótulo no eixo Y: "Pontos" e rótulo no eixo X: "faixa de renda".
| A | A entrega está correta e o conteúdo mostra ou induz uma análise crítica do contexto ou das implicações dos dados e informações coletadas. | Título da figura: "Desempenho no ENEM diminui com o aumento da renda", com rótulo no eixo Y: "Pontos" e rótulo no eixo X: "faixa de renda".

A auto-avaliação será avaliada da seguinte forma:

| Rubrica | Descrição | Exemplo |
| --- | --- | --- | 
| F | Não entregue, entregue fora do prazo, entregue fora do escopo ("fiz outra coisa") | Não entregar |
| D | Entregue no prazo e dentro do escopo, mas somente copia as rubricas que foram apresentadas, sem relacioná-las com o material apresentado, ou apresenta notas sem argumentar o motivo delas. | "Nota A, pois está correto e o conteúdo induz análise crítica do contexto"
| C | Entregue, mas claramente puxa as notas "para cima" ou "para baixo" sem ter argumentos | Entrega é uma figura sem título, mas auto-avaliação diz: "o título da figura está correto"
| A | Entregue, e argumentado corretamente  | "Nota A porque a passagem `de acordo com Fulano et al.` mostra que os resultados obtidos condizem com a literatura", ou "Nota B, porque a figura tem todos os elementos, embora não tenha nenhuma indicação de uma leitura crítica do contexto.


Para atingir cada nível, é necessário cumprir todos os requisitos do nível anterior. Cada uma das entregas será avaliada individualmente e isoladamente. A nota geral do trabalho será a menor entre as notas das entregas.

