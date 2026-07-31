# ap hibrido otimizacao

Dois assuntos avançados que multiplicam o valor do que você construiu: **misturar métodos** num mesmo modelo e **otimizar** de verdade, em vez de só varrer parâmetros.

## Modelos híbridos: o forte do AnyLogic

A grande vantagem do AnyLogic sobre ferramentas de método único é poder **combinar** eventos discretos, dinâmica de sistemas e agentes no mesmo modelo. No nosso hub, por exemplo:

* **Agentes + Eventos Discretos:** dentro do hub, em vez da nossa fila artesanal por mensagens, cada carregador poderia ser um recurso de uma **Process Modeling Library** (blocos `Seize`/`Delay`/`Release`), enquanto os motoristas continuam agentes que se movem pela cidade. O motorista "entra" no fluxo de processos ao chegar e "sai" ao terminar.
* **Agentes + Dinâmica de Sistemas:** um modelo de **estoque e fluxo** poderia governar a adoção de veículos elétricos na cidade (quantos agentes existem ao longo dos anos), enquanto os agentes simulam a operação diária. A saída da DS vira a entrada da população de agentes.

A ponte entre os mundos são funções e variáveis compartilhadas no `Main`. Não há "modo" a selecionar — os três métodos coexistem no mesmo diagrama.

## Optimization: minimizar custo, não só cruzar um limiar

O `Parameters Variation` respondeu "qual o menor `nCarregadores` que atinge 90%". Mas e se o objetivo real for **minimizar o custo total**, equilibrando o investimento em carregadores contra a penalidade por mau serviço? Isso é um problema de **otimização**, e o AnyLogic embute o motor **OptQuest**.

{% stepper %}
{% step %}
## Preparar a função-objetivo

No `Main`, crie uma função `custoTotal()` (retorno `double`) que traduz a decisão em dinheiro:

```java
double custoInfra   = nCarregadores * 25000.0;          // R$ por carregador (CAPEX anualizado)
double faltaServico = max(0, 90 - nivelServico());       // pontos abaixo da meta
double penalidade   = faltaServico * 8000.0;             // R$ por ponto percentual faltante
return custoInfra + penalidade;
```

A penalidade só "acende" quando o serviço cai abaixo de 90%, empurrando a solução para cumprir a meta pelo caminho mais barato.
{% endstep %}

{% step %}
## Criar o experimento Optimization

1. **New > Experiment > Optimization**, top-level agent `Main`, nome `OtimizaHub`.
2. **Objective:** _minimize_ `root.custoTotal()`.
3. **Parameters (variáveis de decisão):** `nCarregadores` como **discrete**, de `1` a `12`, passo `1`.
4. **Constraints (opcional):** você pode adicionar `root.nivelServico() >= 90` como restrição rígida, em vez de (ou além de) usar a penalidade.
5. **Replications:** como o modelo é estocástico, ative **replications** (ex.: 20–50 por avaliação) para o OptQuest comparar médias, não sorte.
{% endstep %}

{% step %}
## Rodar (inclusive na nuvem)

Rode localmente para testar e, para volume, **exporte o experimento de otimização para a AnyLogic Cloud**, do mesmo jeito que fizemos com o `Parameters Variation`. O OptQuest busca de forma inteligente (metaheurística), avaliando muito menos combinações que uma varredura exaustiva — precioso quando há **vários** parâmetros de decisão (ex.: número de carregadores **e** potência de cada um **e** localização de dois hubs).
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Quando usar cada um?** Poucos parâmetros e você quer **ver a curva inteira** → `Parameters Variation`. Muitos parâmetros e você quer **o melhor ponto** segundo um objetivo econômico → `Optimization`. Os dois se complementam: varra para entender, otimize para decidir.
{% endhint %}
