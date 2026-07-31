# analise final

A curva que a nuvem desenhou — **nível de serviço** no eixo vertical, **número de carregadores** no horizontal — é a resposta ao nosso problema. Ela tem um formato característico, em **S deitado / saturação**: cresce rápido no começo e depois se achata perto de 100%.

## Lendo a curva

Percorra a curva da esquerda para a direita e cruze com a linha da meta (**90%**):

| `nCarregadores` | Nível de serviço (ilustrativo) | Leitura                                               |
| :-------------: | :----------------------------: | ----------------------------------------------------- |
|        1        |              \~25%             | Caótico: fila enorme, quase ninguém atendido no prazo |
|        2        |              \~55%             | Ainda muito ruim                                      |
|        3        |              \~78%             | Melhora, mas **abaixo da meta** (era o nosso padrão!) |
|        4        |              \~91%             | **Cruza os 90%** — primeiro valor que atende          |
|        5        |              \~97%             | Folga confortável                                     |
|        6        |              \~99%             | Ganho marginal pequeno                                |
|       7–8       |             \~100%             | Dinheiro jogado fora                                  |

{% hint style="info" %}
Os números acima são **ilustrativos** — os seus vão variar conforme as sementes e eventuais ajustes de parâmetros. O que importa é o **formato** e o **método de leitura**, não os valores exatos.
{% endhint %}

## A decisão de engenharia

O menor número de carregadores que atinge a meta de **90%** é **4** (no exemplo acima). Essa é a resposta ao problema do início do tutorial:

> **O hub deve ter 4 carregadores** para que ao menos 90% das recargas comecem com menos de 10 minutos de espera.

Repare no que a curva ensina para além do número:

* **Rendimentos decrescentes.** Sair de 3 para 4 carregadores muda o jogo (+13 pontos de serviço). Sair de 6 para 7 quase não muda nada. Cada carregador extra rende menos que o anterior.
* **Risco de subdimensionar.** Com 3 carregadores (nosso palpite inicial!), o serviço fica em \~78% — longe da meta. Um palpite "razoável" estava errado, e só a simulação mostrou isso.
* **Custo de superdimensionar.** Instalar 6 quando 4 bastam é enterrar o custo de dois carregadores para ganhar poucos pontos percentuais.

## E se o custo entrar na conta?

Até aqui buscamos o menor `nCarregadores` que cumpre a meta. Mas e se quiséssemos **minimizar o custo total** (equipamento + eventual penalidade por mau serviço) em vez de simplesmente cruzar um limiar? Aí o experimento certo é o **Optimization** (OptQuest), que o AnyLogic também roda na nuvem. Ele está no **Apêndice D**.

Antes disso, um desafio para fixar o raciocínio.
