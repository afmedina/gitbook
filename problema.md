# problema

Uma operadora de mobilidade vai instalar um **hub de recarga** para atender uma frota de **25 veículos elétricos** que circulam por uma região urbana ao longo do dia. Cada carregador custa caro — tanto o equipamento quanto a obra e a conexão elétrica —, então a operadora não quer instalar mais do que o necessário. Por outro lado, se houver carregadores de menos, os motoristas vão formar fila, esperar, reclamar e, no limite, procurar a concorrência.

A pergunta é a clássica pergunta de dimensionamento de recurso, só que agora com sabor de agentes:

> **Quantos carregadores o hub precisa ter para que pelo menos 90% das recargas comecem com menos de 10 minutos de espera na fila?**

{% hint style="info" %}
**Objetivo deste tutorial:** construir um modelo baseado em agentes que permita avaliar o **nível de serviço** (percentual de recargas com espera aceitável), o **tamanho da fila** e a **utilização** dos carregadores, e então determinar o **número de carregadores** que atinge a meta de serviço ao menor custo.
{% endhint %}

## Por que agentes, e não processos?

Você poderia modelar isto como uma fila de eventos discretos (chegadas → fila → recurso → saída), e funcionaria. Mas repare no que torna este problema naturalmente "de agentes":

* Cada motorista tem um **estado interno** (rodando, precisando carregar, indo ao posto, na fila, carregando) e **transita** entre esses estados por conta própria.
* O motorista **não chega** ao sistema de fora: ele já está no sistema o tempo todo, rodando pela cidade, e só _decide_ ir ao hub quando a bateria baixa.
* Há **espaço e movimento**: o tempo até chegar ao hub depende de onde o motorista está.

Isso é exatamente o que a modelagem baseada em agentes descreve bem. Cada motorista será um **agente** com um **statechart**, e a fila no hub vai _emergir_ do comportamento dos 25 agentes disputando um número limitado de carregadores.

## O ciclo de vida de um motorista

Antes de abrir o AnyLogic, fixe na cabeça o ciclo que vamos modelar:

{% stepper %}
{% step %}
## Rodando

O veículo circula pela região, consumindo bateria.
{% endstep %}

{% step %}
## Indo ao posto

Quando a autonomia acaba, o motorista se desloca até o hub.
{% endstep %}

{% step %}
## Na fila

Ao chegar, se todos os carregadores estiverem ocupados, ele espera.
{% endstep %}

{% step %}
## Carregando

Assim que um carregador vaga, ele ocupa e recarrega.
{% endstep %}

{% step %}
## De volta a Rodando

Recarregado, o ciclo recomeça.
{% endstep %}
{% endstepper %}

Guarde esses cinco estados. Eles vão virar, quase literalmente, os cinco estados do nosso statechart.
