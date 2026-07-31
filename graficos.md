# graficos

Vamos montar, no `Main`, um pequeno painel para enxergar o modelo funcionando. Três elementos bastam: o tamanho da fila ao longo do tempo, a distribuição das esperas e o KPI de nível de serviço.

## Time Plot: o tamanho da fila

Da paleta **Analysis**, arraste um **Time Plot** para uma área livre do `Main`. Nas propriedades, adicione uma série de dados (**Data**):

* **Title:** `Fila`
* **Value:** `fila.size()`

Isso desenha, em tempo real, quantos motoristas estão esperando. Você vai ver picos quando vários carros chegam ao hub ao mesmo tempo.

{% hint style="success" %}
**Dica:** adicione uma segunda série no mesmo gráfico com **Value** = `carregadoresOcupados()` para comparar, lado a lado, a ocupação dos carregadores e o tamanho da fila. Quando a fila cresce **e** a ocupação está no máximo, é sinal claro de recurso insuficiente.
{% endhint %}

## Histogram: a distribuição das esperas

Arraste um **Histogram** (paleta **Analysis**) para o `Main`. Em **Data**, aponte para o histograma que alimentamos:

* **Data:** `histEspera`

Agora você vê a "cara" das esperas: idealmente, uma barra alta perto de zero (quase ninguém espera) e uma cauda curta. Se a cauda for longa e gorda, muita gente está esperando muito.

## Um KPI na tela: nível de serviço

Nada como ver o número que importa em letras garrafais. Da paleta **Presentation**, arraste um **Text** para o `Main`. Marque a caixa **Dynamic value** (valor dinâmico) e escreva:

```java
String.format("Nível de serviço: %.1f%%  |  Fila máx: %d", nivelServico(), filaMax)
```

Assim, enquanto o modelo roda, você acompanha o percentual de recargas dentro do limite de espera e a maior fila já observada.

## Rode e observe

Clique em **Run**. Deixe o modelo rodar os 30 dias (vá aumentando a velocidade de execução com o controle de velocidade, ou desligue a animação para terminar em segundos). Ao final, anote o **nível de serviço** com `nCarregadores = 3`.

Provavelmente você vai encontrar um nível de serviço **abaixo** dos 90% desejados. Três carregadores não bastam. Mas quantos bastam? Poderíamos ficar mudando o `3` na mão e rodando de novo, de novo, de novo... ou deixar a nuvem fazer isso por nós. É a Parte IV.
