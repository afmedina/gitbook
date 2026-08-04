# O nosso primeiro agente: motorista

Antes de desenhar o comportamento (o statechart), vamos dar ao `Motorista` os “números” que descrevem seu ciclo. Abra a aba do agente **`Motorista`**.

## Parâmetros do Motorista

Da paleta **Agent**, arraste três **Parameter** para dentro do diagrama do `Motorista` e configure cada um:

| Nome              | Tipo   | Valor padrão                | Significado                                                   |
| ----------------- | ------ | --------------------------- | ------------------------------------------------------------- |
| `autonomiaMin`    | double | `triangular(240, 300, 420)` | Tempo, em minutos, que o veículo roda até precisar recarregar |
| `tempoRecargaMin` | double | `triangular(30, 45, 75)`    | Duração da recarga, em minutos                                |
| `velocidadeKmh`   | double | `30`                        | Velocidade de deslocamento do veículo                         |

{% hint style="info" %}
Colocamos `triangular(min, moda, max)` **como valor padrão do parâmetro**. Assim, cada motorista sorteia o seu próprio tempo toda vez que o parâmetro é lido — e não teremos 25 veículos com exatamente a mesma autonomia. Distribuições triangulares são ótimas quando você tem “mínimo, mais provável e máximo” na cabeça, mas não tem dados finos.
{% endhint %}

## Variáveis do Motorista

Vamos precisar de uma variável para cronometrar quanto tempo o motorista esperou na fila. Arraste uma **Variable** da paleta **Agent** para dentro do `Motorista`:

| Nome            | Tipo   | Valor inicial | Para quê                                                                    |
| --------------- | ------ | ------------- | --------------------------------------------------------------------------- |
| `tEntrouNaFila` | double | `0`           | Guarda o instante em que o motorista entrou na fila, para calcular a espera |

Só isso por enquanto no `Motorista`. Toda a inteligência dele vai morar no **statechart**, que é a próxima página — e a mais importante do tutorial.

{% hint style="success" %}
**Dica:** mantenha os nomes exatamente como estão aqui, inclusive maiúsculas e minúsculas. O AnyLogic gera código Java a partir desses nomes, e mais adiante vamos escrever expressões que os referenciam. `Autonomia` e `autonomia` são coisas diferentes para o Java.
{% endhint %}
