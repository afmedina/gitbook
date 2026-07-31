# recurso fila

O statechart do motorista vai precisar conversar com o "mundo": perguntar se há carregador livre, entrar na fila, ser chamado quando vagar. Toda essa infraestrutura mora no **`Main`**. Vamos montá-la agora, antes do statechart, para que ele já encontre tudo pronto.

Abra a aba do **`Main`**.

## Parâmetros do Main

Arraste três **Parameter** para o `Main`:

| Nome            | Tipo   | Valor padrão | Significado                                                     |
| --------------- | ------ | ------------ | --------------------------------------------------------------- |
| `nMotoristas`   | int    | `25`         | Tamanho da frota                                                |
| `nCarregadores` | int    | `3`          | **A variável de decisão** — o que vamos dimensionar             |
| `limiteEspera`  | double | `10`         | Espera máxima aceitável na fila, em minutos (a meta de serviço) |

Agora conecte o tamanho da população ao parâmetro: clique na população **`motoristas`** (no diagrama do `Main`) e, na propriedade **Initial number of agents**, coloque `nMotoristas`.

## Variáveis do Main

Arraste **Variable** para o `Main` e crie:

| Nome                 | Tipo   | Valor inicial   | Para quê                                          |
| -------------------- | ------ | --------------- | ------------------------------------------------- |
| `carregadoresLivres` | int    | `nCarregadores` | Quantos carregadores estão livres agora           |
| `filaMax`            | int    | `0`             | Maior fila observada                              |
| `nRecargas`          | int    | `0`             | Total de recargas concluídas                      |
| `nDentroSLA`         | int    | `0`             | Recargas que começaram dentro do limite de espera |
| `hubX`               | double | `2500`          | Coordenada X do hub no espaço                     |
| `hubY`               | double | `1750`          | Coordenada Y do hub no espaço                     |

{% hint style="info" %}
Repare que `carregadoresLivres` **começa** valendo `nCarregadores`. Como o valor inicial de uma variável pode ser uma expressão, não precisamos de nenhum código de inicialização: quando o experimento mudar `nCarregadores`, os livres começam certos automaticamente.
{% endhint %}

## A fila: uma coleção de motoristas

Como vários motoristas podem esperar ao mesmo tempo, precisamos de uma **fila de espera**. Arraste um **Collection** (paleta **Agent**) para o `Main` e configure:

* **Name:** `fila`
* **Collection class:** `LinkedList`
* **Element class:** `Motorista`

Uma `LinkedList` nos dá ordem de chegada (FIFO) de graça: quem entra primeiro, é atendido primeiro.

## O "cérebro" do hub: três funções

Aqui está o pulo do gato deste modelo. Um carregador que vaga precisa **avisar** quem está esperando. No AnyLogic, uma transição por **condição** (`carregadoresLivres > 0`) _não_ é reavaliada quando **outro** agente muda de estado — ela só reage a eventos do próprio agente. Se confiássemos numa condição, a fila travaria: o motorista que libera o carregador muda o `Main`, mas os que esperam não "percebem".

A solução idiomática em modelagem baseada em agentes é a **comunicação por mensagem**. O hub funciona como um pequeno controlador: mantém a fila, e sempre que há vaga, **chama** o próximo enviando-lhe uma mensagem. Vamos criar três funções no `Main`.

{% stepper %}
{% step %}
## `entrarNaFila`

Chamada pelo motorista quando ele chega ao hub. Um argumento `m` do tipo `Motorista`; sem retorno (`void`):

```java
fila.add(m);
filaMax = max(filaMax, fila.size());
tentarAtender();
```
{% endstep %}

{% step %}
## `tentarAtender`

O coração do controlador. Sem argumentos, sem retorno (`void`):

```java
while (carregadoresLivres > 0 && !fila.isEmpty()) {
    Motorista m = fila.remove(0);   // o primeiro da fila (FIFO)
    carregadoresLivres--;           // reserva um carregador para ele
    send("vaga", m);                // avisa o motorista: pode carregar!
}
```
{% endstep %}

{% step %}
## `liberarCarregador`

Chamada pelo motorista ao terminar a recarga. Sem argumentos, sem retorno (`void`):

```java
carregadoresLivres++;
tentarAtender();   // liberou uma vaga: talvez alguém da fila possa entrar
```
{% endstep %}
{% endstepper %}

{% hint style="success" %}
**Dica:** repare como a função `tentarAtender()` é o único lugar que reserva um carregador (`carregadoresLivres--`) e o único que decide quem entra. Concentrar a regra num só ponto evita bugs. Ela é chamada tanto quando alguém **chega** (`entrarNaFila`) quanto quando alguém **sai** (`liberarCarregador`) — os dois únicos momentos em que a situação da fila pode mudar.
{% endhint %}

## O indicador de nível de serviço

Por fim, uma função que devolve o nosso KPI. Arraste mais uma **Function** para o `Main`, com **nome** `nivelServico`, **tipo de retorno** `double`:

```java
return nRecargas == 0 ? 100.0 : 100.0 * nDentroSLA / nRecargas;
```

É o percentual de recargas que começaram dentro do limite de espera. É esse número que queremos manter **≥ 90%**.

Com o `Main` preparado, podemos finalmente dar vida ao motorista.
