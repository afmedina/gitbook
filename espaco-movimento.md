# espaco movimento

Até aqui nossos motoristas têm comportamento, mas não têm **onde** estar. Vamos dar a eles um espaço 2D para circular e um hub físico para onde ir. É o que diferencia, visualmente e conceitualmente, um modelo de agentes de um fluxograma de processos: os agentes **existem no espaço** e se **movem**.

## 1. O ambiente contínuo do Main

Abra o **`Main`**. Da paleta **Space Markup**, você vai usar o espaço contínuo (o `Main` já tem um ambiente padrão). Vamos apenas dimensioná-lo. Clique numa área vazia do `Main` e, nas propriedades da janela do agente, ajuste o tamanho da área de desenho — ou simplesmente trabalhe numa região de aproximadamente **5000 × 3500** (pensaremos nessas unidades como **metros**: uma região urbana de 5 km por 3,5 km).

{% hint style="info" %}
O AnyLogic tem três tipos de espaço: **contínuo** (o carro pode estar em qualquer coordenada), **em rede/GIS** (movimento por ruas reais) e **discreto** (grade). Para este tutorial usamos o **contínuo**, o mais simples. O Apêndice B mostra como levar o mesmo modelo para um mapa GIS de verdade.
{% endhint %}

## 2. O hub no espaço

Vamos marcar visualmente onde fica o hub. Da paleta **Space Markup**, arraste um **Point Node** (ou, se preferir só o visual, um ícone da paleta **Pictures**) para a posição central da área, aproximadamente em **(2500, 1750)** — exatamente as coordenadas `hubX` e `hubY` que definimos no `Main`. Renomeie-o para `hub`.

{% hint style="success" %}
**Dica:** para o hub ficar bonito, coloque também um texto "⚡ HUB" ao lado. A animação não muda os resultados, mas ajuda muito na hora de apresentar o modelo para a turma (ou para o cliente).
{% endhint %}

## 3. Espalhar os motoristas pela cidade

Clique na população **`motoristas`** e, nas propriedades, procure a seção de **posição inicial** (Initial location). Escolha **Random** dentro da área do ambiente. Assim, ao iniciar a simulação, os 25 carros já aparecem espalhados pela região, e não empilhados na origem.

## 4. A velocidade dos carros

Abra o agente **`Motorista`**. Nas propriedades do agente, na seção **Movement / Space and network**, defina a **Speed (velocidade)** como `velocidadeKmh`, escolhendo a unidade **kilometers per hour**. Com o hub no centro e a cidade com 5 km de lado, um carro leva tipicamente de 5 a 12 minutos para chegar ao hub — o que dá ao deslocamento um peso realista no problema.

## 5. Fazer os carros circularem

### Enquanto está `Rodando`: perambular pela cidade

Abra o statechart do `Motorista`. Selecione o estado **`Rodando`** e escreva na **Entry action**:

```java
moveTo(uniform(0, 5000), uniform(0, 3500));
```

Isso manda o carro para um ponto aleatório assim que ele entra em `Rodando`. Mas queremos que ele continue perambulando enquanto não precisa recarregar.

{% stepper %}
{% step %}
## Adicione uma transição interna em `Rodando`

Adicione uma **transição interna**: uma seta que sai e volta ao próprio `Rodando`.

* **Triggered by:** `Agent arrival`
* **Action:**

```java
moveTo(uniform(0, 5000), uniform(0, 3500));
```
{% endstep %}

{% step %}
## Continue perambulando pela cidade

Sempre que o carro chega ao destino, sorteia outro e segue. Ele fica vagando — até o `Timeout` de `autonomiaMin` interrompê-lo e mandá-lo para `IndoAoPosto`.
{% endstep %}
{% endstepper %}

### Ao ir ao posto: rumo ao hub

Selecione o estado **`IndoAoPosto`** e escreva na **Entry action**:

```java
moveTo(main.hubX, main.hubY);
```

Agora o gatilho **Agent arrival** da transição `IndoAoPosto → NaFila` faz sentido: quando o carro chega às coordenadas do hub, ele entra na fila.

{% hint style="info" %}
**Por que a transição interna em `Rodando`?** Sem ela, o carro iria a um ponto e ficaria parado lá esperando a bateria acabar. Com ela, ele circula continuamente. É um detalhe de animação, mas também deixa o tempo de deslocamento ao hub mais variável e realista, porque o carro pode estar em qualquer lugar quando a bateria baixa.
{% endhint %}

## Hora de um teste rápido

O modelo já roda. Clique em **Run** (o botão verde) e observe: os carros circulam, de tempos em tempos convergem para o hub, alguns esperam, depois voltam a se espalhar. Se você definiu `nCarregadores = 3`, provavelmente vai ver uma fila se formar no hub em certos momentos — que é justamente o problema que viemos resolver.

Nas próximas páginas vamos **medir** tudo isso: nível de serviço, tamanho da fila e utilização.
