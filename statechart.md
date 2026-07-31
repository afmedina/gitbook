# statechart

Chegamos ao coração do modelo. O **statechart** (diagrama de estados) descreve, para **um** motorista, os cinco momentos do seu ciclo e as regras que o fazem passar de um para o outro. Como são 25 motoristas rodando o mesmo statechart, cada um com seus próprios sorteios de tempo, o comportamento do hub — inclusive a fila — vai _emergir_ dessa multidão.

Abra a aba do agente **`Motorista`**.

{% stepper %}
{% step %}
## Estados e ponto de entrada

Da paleta **Statechart**, arraste para o diagrama do `Motorista`:

* Um **Statechart Entry Point** (a bolinha com a seta).
* Quatro **State** (os retângulos arredondados). Nomeie-os, de cima para baixo: `Rodando`, `IndoAoPosto`, `NaFila`, `Carregando`.

Ligue o **Entry Point** ao estado `Rodando` — todo motorista começa rodando.

{% hint style="info" %}
Nomes de estados começam com letra **maiúscula** por convenção (eles viram constantes Java). Vamos referenciá-los em código, então mantenha exatamente `Rodando`, `IndoAoPosto`, `NaFila` e `Carregando`.
{% endhint %}
{% endstep %}

{% step %}
## As transições e seus gatilhos

Desenhe as **Transition** (as setas) ligando os estados no ciclo `Rodando → IndoAoPosto → NaFila → Carregando → Rodando`. Configure o **gatilho (Triggered by)** e as **ações (Action)** de cada uma exatamente assim.

### `Rodando → IndoAoPosto` — a bateria acabou

* **Triggered by:** `Timeout`
* **Timeout:** `autonomiaMin`

Como `autonomiaMin` tem uma distribuição triangular como padrão, cada passagem por `Rodando` sorteia uma autonomia diferente. Quando o tempo esgota, o motorista precisa recarregar.

### `IndoAoPosto → NaFila` — cheguei ao hub

* **Triggered by:** `Agent arrival`

Este gatilho dispara quando o agente **termina o seu deslocamento** (o `moveTo` que vamos configurar na próxima página). Ou seja: assim que o carro chega ao hub, ele entra na fila.

### `NaFila → Carregando` — fui chamado

* **Triggered by:** `Message`
* **Message type:** `String`
* **Fire the transition:** _when a specific message received_ → `"vaga"`
* **Action:**

```java
double espera = time() - tEntrouNaFila;
main.nRecargas++;
if (espera <= main.limiteEspera) main.nDentroSLA++;
```

Esta transição só dispara quando o motorista recebe a mensagem `"vaga"` enviada pela função `tentarAtender()` do hub. Nesse instante calculamos **quanto ele esperou** e registramos se a espera ficou dentro do limite de serviço.

### `Carregando → Rodando` — recarreguei, de volta à rua

* **Triggered by:** `Timeout`
* **Timeout:** `tempoRecargaMin`
* **Action:**

```java
main.liberarCarregador();
```

Ao terminar a recarga, o motorista **libera** o carregador — e a função `liberarCarregador()` já chama `tentarAtender()`, que pode puxar o próximo da fila. O ciclo recomeça.
{% endstep %}

{% step %}
## As ações do estado `NaFila`

Selecione o estado **`NaFila`** e, na propriedade **Entry action**, escreva:

```java
tEntrouNaFila = time();
main.entrarNaFila(this);
```

Guardamos o instante de entrada, para medir a espera, e nos anunciamos ao hub com `entrarNaFila(this)` — onde `this` é o próprio motorista.

Se houver carregador livre naquele momento, `tentarAtender()` vai nos enviar `"vaga"` na mesma hora, e a espera será praticamente zero. Se não houver, ficamos aguardando a mensagem.

{% hint style="success" %}
**Dica:** o `send("vaga", m)` do hub não é entregue no meio da nossa ação de entrada — o AnyLogic o agenda como um evento no mesmo instante de tempo, logo após a ação terminar. Por isso o motorista já estará seguramente no estado `NaFila` quando a mensagem chegar. É um padrão robusto: **anuncie-se e espere a resposta**.
{% endhint %}
{% endstep %}
{% endstepper %}

## Como o modelo se comporta agora

Pare um instante e leia o statechart como uma história: o carro roda por um tempo, fica sem bateria, vai ao hub, se anuncia. Se há vaga, carrega; senão, entra numa fila que o hub administra por ordem de chegada. Ao terminar, libera a vaga — que imediatamente pode ser ocupada por quem esperava — e volta a rodar.

Falta só uma coisa para o modelo andar (literalmente): o **espaço** e o **movimento**. É a próxima página.
