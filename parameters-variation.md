# parameters variation

O experimento **Parameters Variation** roda o modelo muitas vezes, varrendo faixas de parâmetros e repetindo réplicas para cada combinação. É a ferramenta certa para o nosso dimensionamento.

{% stepper %}
{% step %}
## Criar o experimento

Na árvore do projeto, clique com o botão direito sobre o modelo (o item do topo) e escolha **New > Experiment**. Na janela:

* **Experiment type:** `Parameter variation`
* **Name:** `DimensionamentoHub`
* **Top-level agent:** `Main`
* **Finish**.
{% endstep %}

{% step %}
## Definir a varredura de `nCarregadores`

Nas propriedades do experimento, seção **Parameters**:

* Encontre `nCarregadores` e marque-o como **Range** (faixa).
* **Min:** `1` · **Max:** `8` · **Step:** `1`
* Deixe `nMotoristas` e `limiteEspera` como **Fixed** (fixos) nos valores padrão (`25` e `10`).

Isso gera 8 valores de `nCarregadores` (de 1 a 8).
{% endstep %}

{% step %}
## Adicionar réplicas

Como o modelo é estocástico, cada valor precisa de **várias réplicas**. Ainda nas propriedades:

* Marque **Randomness → Random seed** (semente aleatória — cada réplica com sorteios diferentes).
* Em **Replications per iteration** (ou "número de réplicas"), coloque `30`.

Resultado: 8 valores × 30 réplicas = **240 execuções**. Na nuvem, elas rodam em paralelo.

{% hint style="success" %}
**Dica:** 30 réplicas é um bom ponto de partida. Se os intervalos de confiança ainda ficarem largos perto da meta de 90%, aumente para 50 ou 100 — a nuvem aguenta.
{% endhint %}
{% endstep %}

{% step %}
## Coletar a saída de cada réplica

Precisamos guardar, de cada execução, o **nível de serviço**. Na aba **Java actions** do experimento, no campo **After simulation run** (executado ao fim de cada réplica), registre o resultado. Uma forma simples é acumular num **HistogramData** ou **DataSet** por valor de `nCarregadores`; a maneira mais direta, porém, é deixar a **nuvem** fazer o gráfico para nós (próximo passo).

Para isso, garanta que o experimento **exponha** o indicador. Em muitos fluxos, basta ter a função `nivelServico()` no `Main`: a nuvem permite escolhê-la como saída a ser plotada contra o parâmetro variado.
{% endstep %}

{% step %}
## Exportar o experimento para a nuvem e rodar

1. **File > Export > to AnyLogic Cloud**, selecionando desta vez o experimento **`DimensionamentoHub`**.
2. No navegador, abra o experimento. Configure o eixo:
   * **X (parâmetro):** `nCarregadores`
   * **Y (saída):** `nivelServico` (média das réplicas)
3. Clique em **Run**. Acompanhe as 240 execuções sendo processadas.

Ao final, a nuvem desenha a curva **nível de serviço × número de carregadores**, com a média e a dispersão das réplicas.

Na próxima página, lemos essa curva e tomamos a decisão.
{% endstep %}
{% endstepper %}
