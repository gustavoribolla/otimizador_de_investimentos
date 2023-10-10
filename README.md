# APS 3 - Otimização de Portifolio de Investimentos

## Disclaimer

Nesta atividade, vamos usar o que aprendemos aqui para otimizar uma carteira de investimentos. Um disclaimer importante é que você provavelmente não deveria usar somente isso para compor sua própria carteira de investimentos. O motivo de usarmos este procedimento específico é que ele cabe bem no conteúdo que temos aprendido e, embora seja uma estratégia válida e importante, ela não observa vários fatores que também são relevantes na escolha de um portifolio. Se você quer fazer um portifolio de investimentos, estude mais sobre o assunto, incluindo outros critérios para a escolha dos componentes do portifolio. Se, mesmo assim, você quiser testar o seu portifolio em ação, use um [simulador de bolsas de valores](https://www.google.com/search?q=simulador+de+bolsa+de+valores&rlz=1C1DVJR_enCA884BR885&sxsrf=AB5stBhxFF_UxynuTjUhCA8kLrpPCRFJoA%3A1691500926024&ei=fkHSZMuOAdG95OUPn_ed-AI&gs_ssp=eJzj4tVP1zc0TDczTavKzSgwYPSSLc7MLc1JTMkvUkhJVUjKzylOBDHKEnPyi1KLAV2CD70&oq=simulador+de+bolsa+d&gs_lp=Egxnd3Mtd2l6LXNlcnAiFHNpbXVsYWRvciBkZSBib2xzYSBkKgIIADIIEC4YgAQYywEyCBAAGIAEGMsBMggQABiABBjLATIFEAAYgAQyBRAAGIAEMgUQABiABDIGEAAYFhgeMgYQABgWGB4yBhAAGBYYHjIGEAAYFhgeMhcQLhiABBjLARiXBRjcBBjeBBjgBNgBA0iHH1CpBliGF3ADeAGQAQCYAaQBoAHUE6oBBDAuMjC4AQPIAQD4AQHCAgoQABhHGNYEGLADwgIKEAAYigUYsAMYQ8ICDhAAGOQCGNYEGLAD2AEBwgIWEC4YigUYxwEY0QMYyAMYsAMYQ9gBAsICBxAjGIoFGCfCAgQQIxgnwgIHEAAYigUYQ8ICDRAuGIoFGMcBGNEDGEPCAgcQLhiKBRhD4gMEGAAgQYgGAZAGEroGBggBEAEYCboGBggCEAEYCLoGBggDEAEYFA&sclient=gws-wiz-serp).

## Introdução

Vários fundos de investimentos usam ferramentas computacionais para definir seus portifolios. Uma ferramenta muito conhecida para isso é o Índice Sharpee. Esse índice foi [proposto nos anos 60](https://www.jstor.org/stable/2351741) e parte das seguintes ideias:

## Retorno relativo

Um portifolio tem um retorno relativo igual a:

$$
R =\frac{\text{valor}_f - \text{valor}_{0}}{\text{valor}_{0}} = \frac{\text{valor}_f}{\text{valor}_{0}} -1.
$$

Por simplicidade, vamos assumir uma estratégia do tipo "buy-and-hold", isto é, $\text{valor}_0$ é o valor do portifólio no dia inicial e $\text{valor}_f é o valor do portifólio no dia final.

## Aumentar ganhos, diminuir riscos

O índice sharpee é a divisão da esperança do retorno relativo pelo desvio padrão desse mesmo retorno, isto é:

$$
S = \frac{R}{\sigma_R},
$$
onde $\sigma_R$ é o desvio padrão do retorno relativo diário durante o período em que o retorno total foi calculado. O retorno relativo diário pode ser calculado usando a função `.pct_change()` do Pandas.

O índice sharpee aumenta quando a esperança do retorno aumenta ou quando seu desvio padrão diminui, isto é, é um índice que 

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

onde $\text{Cov}(x,y)$ é a covariância entre $x$ e $y$, que é uma medida análoga à correlação, exceto por ser multiplicada pelos desvios padrões de ambas as medidas. A covariância tende a zero quando a correlação entre as variáveis correspondentes tende a zero - e por isso ignoramos esse fator quando trabalhamos com variáveis independentes.

Lembra-se do caso dos seguros, em que o desvio padrão cresce muito mais devagar que a média dos retornos quando aumentamos o número de clientes? Algo parecido acontece com ações. Ao combinarmos em nosso portifólio ações cujos retornos são descorrelacionados, o desvio padrão tende a crescer mais devagar que a média.

## Tutorial prático

Deixei um [tutorial sobre o índice Sharpee](acoes.ipynb) no repositório. À partir dele, todos esses conceitos podem ficar mais claros.

## Enunciado da atividade

Nesta atividade, você vai implementar e executar um algoritmo para encontrar um portifolio de ações baseado no índice Sharpee. Para isso, use dados disponibilizados pela B3, ou a biblioteca do Yahoo Finance.

1. Escolha pelo menos 20 ações candidatas da bolsa brasileira (B3). Para isso, use o critério que preferir.
2. Inicie com uma ação à sua escolha e coloque em seu portifolio
3. Calcule o índice Sharpee de seu portifolio (que inicialmente tem só uma ação)
4. Calcule a correlação entre os retornos de seu portifolio e os retornos das ações candidatas restantes
5. Adicione em seu portifolio a ação cujos retornos diários são menos correlacionados a ele. Assuma que você tem um valor fixo para cada elemento do portifolio (por exemplo: tem sempre R$1000,00 investido em cada ação nova).
6. Volte ao passo 3 até encontrar 4 ações para compor seu portifolio.
7. Observe as ações que você escolheu. Em que setores as empresas operam? São setores diferentes?
8. Mostre numa figura como o Índice Sharpee se comporta a cada adição de uma nova ação.
9. Escreva um pequeno texto (até 500 palavras, mas pode ser menos) explicando porque o seu portifolio faz sentido. Para isso, use o que aprendemos sobre estatística, e também o que você souber sobre economia e investimentos. Se necessário, cite fontes.


# Entregas esperadas

Você deve entregar, via Blackboard, um arquivo PDF (uma entrega por grupo!) contendo:

1. Nome do grupo
2. Título do trabalho, que deve ser algo como "Otimizando portifólio de ações com índice Sharpee" (parafraseie isso!)
3. A figura gerada no ítem 8.
4. O texto gerado no ítem 9.
5. Uma auto-avaliação da figura e do texto, justificando as notas que foram auto-atribuídas.

# Rubricas e avaliação

Cada um dos ítens (exceto a auto-avaliação) será avaliado da seguinte forma:

| Rubrica | Descrição                                                                                                                                                                                                                                                                                                                                                            | Exemplo                                                                                                                                                              |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| F       | Não entregue, entregue fora do prazo, entregue fora do escopo ("fiz outra coisa")                                                                                                                                                                                                                                                                                     | Não entregar                                                                                                                                                        |
| E       | Entregue no prazo e dentro do escopo, mas a entrega tem um ou mais ítens faltando ou só é compreensível se for acompanhado deste enunciado (por exemplo, ao dizer "resposta ao ítem 2" ao invés de ter um título explicativo) ou está ilegível (exemplo: textos sem sentido ou figuras muito grandes / muito pequenas ou em resolução baixa)                | Colocar no título de uma figura: "ítem 1a"                                                                                                                         |
| D       | A entrega tem falhas graves de coesão ou coerência que impedem sua compreensão, ou faltam elementos essenciais (título, legenda, rótulos nos eixos), ou o texto deixa de indicar o que são ideias originais e o que são ideias retiradas de outras fontes, ou as ideias/dados são mostrados de forma embaralhada, sem guiar o leitor a um fluxo de pensamento. | Em um texto: "Existe impacto da renda na educação. Isso porque a renda pode estar ligada a condições de estudo. O estudo é essencial para formar seres humanos" |
| C       | A entrega não tem falhas graves. Todas as fontes de ideias são indicadas. A entrega traz ao menos um elemento errado ou ao menos um elemento supérfluo (que não contribui para a mensagem passada).                                                                                                                                                                | Em um texto: "Essa é uma fonte de 100W (Watts, unidade batizada em homenagem a James Watt, inventor da máquina a vapor")                                           |
| B       | A entrega tem todos os elementos necessários para passar a mensagem, a mensagem é clara, e não há elementos supérfluos.                                                                                                                                                                                                                                           | Título da figura: "Média de desempenho no ENEM 2022 por disciplina por faixa de renda", com rótulo no eixo Y: "Pontos" e rótulo no eixo X: "faixa de renda".     |
| A       | A entrega está correta e o conteúdo mostra ou induz uma análise crítica do contexto ou das implicações dos dados e informações coletadas.                                                                                                                                                                                                                      | Título da figura: "Desempenho no ENEM diminui com o aumento da renda", com rótulo no eixo Y: "Pontos" e rótulo no eixo X: "faixa de renda".                       |

A auto-avaliação será avaliada da seguinte forma:

| Rubrica | Descrição                                                                                                                                                                                  | Exemplo                                                                                                                                                                                                                                               |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| F       | Não entregue, entregue fora do prazo, entregue fora do escopo ("fiz outra coisa")                                                                                                           | Não entregar                                                                                                                                                                                                                                         |
| D       | Entregue no prazo e dentro do escopo, mas somente copia as rubricas que foram apresentadas, sem relacioná-las com o material apresentado, ou apresenta notas sem argumentar o motivo delas. | "Nota A, pois está correto e o conteúdo induz análise crítica do contexto"                                                                                                                                                                        |
| C       | Entregue, mas claramente puxa as notas "para cima" ou "para baixo" sem ter argumentos                                                                                                        | Entrega é uma figura sem título, mas auto-avaliação diz: "o título da figura está correto"                                                                                                                                                      |
| A       | Entregue, e argumentado corretamente                                                                                                                                                         | "Nota A porque a passagem `de acordo com Fulano et al.` mostra que os resultados obtidos condizem com a literatura", ou "Nota B, porque a figura tem todos os elementos, embora não tenha nenhuma indicação de uma leitura crítica do contexto. |

Para atingir cada nível, é necessário cumprir todos os requisitos do nível anterior. Cada uma das entregas será avaliada individualmente e isoladamente. A nota geral do trabalho será a menor entre as notas das entregas.

## Sobre o uso de IA (ChatGPT, etc.)

ChatGPT e outros modelos de linguagem são ferramentas de produtividade importantes e relevantes. Por outro lado, seu uso indiscriminado pode se tornar um ruído no processo de produção e feedback das atividades, pelos seguintes motivos:

1. O texto produzido é um *proxy* para o entendimento do aluno. Analisando o texto, queremos entender quais são os *gaps* de aprendizado que acontecem, de forma que possamos planejar intervenções pertinentes.
2. O feedback fornecido parte do pressuposto de que o texto foi o melhor trabalho possível apresentado pelo aluno. Nesse contexto, o *feedback* tem a intenção de revisar e proporcionar ao aluno uma visão externa com o intuito de melhorar ou refinar sua compreensão e sua expressão sobre o tema. Fornecer *feedback* para um texto gerado por máquina não tem esse efeito, porém é tão ou mais trabalhoso que dar *feedback* para textos feitos por humanos.

Pensando nisso, vamos adotar as seguintes regras para o uso de ChatGPT e IAs generativas de forma geral para a redação de textos:

1. Caso o grupo decida usar ChatGPT ou equivalente para qualquer parte de sua redação, deve **obrigatoriamente** adicionar uma nova seção anexa ao relatório em que mostra todos os prompts e respostas do ChatGPT (ou todas as interações equivalentes).
2. Também, para cada trecho que use IA, o grupo deverá **explicitamente** justificar porque o trecho foi considerado correto. Justificativas vagas como "achamos pertinente", "condiz com a matéria", etc., não serão aceitas. Justificativas válidas são aquelas que associam explicitamente o texto fornecido pela IA a uma equação, observação ou raciocínio pertinentes ao texto.
3. Neste anexo citado e nas justificativas, não é permitido usar nenhum tipo de IA.
4. O uso de IA sem a citação adequada ou sem o anexo (qualquer um deles, ou ambos simultaneamente), se detectado, será considerado como cópia sem citação de fonte, e, portanto, violação do código de ética.


Para atingir cada nível, é necessário cumprir todos os requisitos do nível anterior. Cada uma das entregas será avaliada individualmente e isoladamente. A nota geral do trabalho será a **menor** entre as notas das entregas.

Caso haja interações com IAs, elas serão avaliadas de acordo com a seguinte rubrica:

| Rubrica           | Descrição                                                                                                                                                                                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Código de ética | Foi identificado o uso de Inteligência Artificial, mas o texto não demonstra clara e diretamente que uma ferramenta foi utilizada ou como ela foi utilizada.                                                                                                            |
| F                 | Somente colocou o enunciado da atividade como entrada da IA e fez alterações mínimas no resultado apresentado. Toda ou a maior parte da informação relevante do trabalho foi gerada pela IA.                                                                        |
| E                 | Trouxe tópicos específicos à IA, mas as conexões lógicas foram todas ou em sua maioria trazidas pela IA. OU: Uma ou mais informações inventadas pela IA (alucinações) foram transportadas para o texto sem crítica.                                             |
| D                 | O grupo trouxe alguns elementos relevantes para a IA, mas todas ou a maioria das conexões lógicas foram feitas pela própria IA.                                                                                                                                        |
| C                 | O grupo trouxe a maioria dos elementos relevantes para a IA, mas a IA ainda foi responsável por trazer ao menos um elemento essencial ao trabalho.                                                                                                                       |
| B                 | IA foi usada para frasear raciocínios, mas todos os elementos relevantes para a construção dos raciocínios foram fornecidas pelo grupo de trabalho.                                                                                                                   |
| A                 | IA foi usada apenas para correções estilísticas, e todo o conteúdo relevante e todas as linhas de raciocínio foram feitas pelo grupo de trabalho, OU a IA foi usada somente para obter ideias sobre o assunto e o grupo escreveu todo o conteúdo com suas palavras. |
