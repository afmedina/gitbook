# coleta resultados

Um modelo que só anima carros é bonito, mas inútil para decidir. Precisamos transformar o comportamento em **números**. Já criamos, no `Main`, os contadores `nRecargas`, `nDentroSLA`, `filaMax` e a função `nivelServico()`. Vamos agora acrescentar a distribuição das esperas e preparar a execução.

## O histograma das esperas

Arraste um **Histogram Data** (paleta **Analysis**) para o `Main` e nomeie-o `histEspera`. Ele acumula valores e monta a distribuição automaticamente.

Agora volte ao statechart do **`Motorista`**, abra a transição **`NaFila → Carregando`** e **acrescente uma linha** à ação, para alimentar o histograma com cada espera medida:

```java
double espera = time() - tEntrouNaFila;
main.nRecargas++;
if (espera <= main.limiteEspera) main.nDentroSLA++;
main.histEspera.add(espera);        // ← nova linha
```

## Utilização dos carregadores

Para acompanhar quantos carregadores estão ocupados ao longo do tempo, crie no `Main` uma **Function** simples chamada `carregadoresOcupados`, tipo de retorno `int`:

```java
return nCarregadores - carregadoresLivres;
```

Vamos plotá-la na próxima página. A **utilização média** (fração do tempo em que os carregadores ficam ocupados) é um ótimo indicador de sobra ou falta de recurso: perto de 100% indica gargalo; muito baixa indica desperdício.

## Configurando a execução

Abra o experimento **Simulation** (na árvore do projeto, em **Experiments**). Nas propriedades:

* **Model time → Stop:** _Stop at specified time_
* **Stop time:** `43200` (equivale a **30 dias** em minutos)

{% hint style="info" %}
**Warm-up (aquecimento):** no instante zero, todos os carros estão cheios e o hub, vazio — uma situação artificial. Os primeiros dias carregam esse viés. Para um número mais limpo, você pode descartar as estatísticas do início: crie um **Event** único disparando em, digamos, `2880` (2 dias) que zera `nRecargas`, `nDentroSLA`, `filaMax` e chama `histEspera.reset()`. É a mesma técnica de _warm-up_ do volume de Modelagem de Processos. Para a aula de 100 minutos, isso é opcional.
{% endhint %}

Com os indicadores no lugar, vamos montar a tela de resultados.
