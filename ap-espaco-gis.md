# ap espaco gis

Usamos o espaço **contínuo**, o mais simples: cada carro tem uma coordenada (x, y) e anda em linha reta até o destino. O AnyLogic oferece dois outros tipos de espaço, mais realistas.

## Os três tipos de espaço

| Espaço             | Como o agente se move                       | Quando usar                                                           |
| ------------------ | ------------------------------------------- | --------------------------------------------------------------------- |
| **Contínuo**       | linha reta entre coordenadas                | leiaute abstrato, chão de fábrica, "cidade" estilizada (o nosso caso) |
| **Rede (network)** | por caminhos e nós definidos (paths, nodes) | armazéns, hospitais, layouts com corredores                           |
| **GIS**            | por ruas reais de um mapa                   | logística urbana, mobilidade, entregas                                |

## Movimento: variações do `moveTo`

* `moveTo(x, y)` — para um ponto no espaço contínuo (usamos este).
* `moveTo(node)` — para um nó de rede ou GIS.
* `moveTo(agente)` — para a posição de outro agente (perseguir, encontrar).
* `jumpTo(x, y)` — teleporta (sem animação de deslocamento).
* Propriedades de **Speed** e o gatilho **Agent arrival** funcionam igual nos três espaços.

## Levando o hub para um mapa GIS de verdade

{% stepper %}
{% step %}
### No `Main`, adicione um mapa GIS

Arraste um **GIS Map** (paleta **Space Markup**). Navegue até a sua cidade e ajuste o zoom.
{% endstep %}

{% step %}
### Marque a localização do hub

Marque a localização do hub com um **GIS Point** sobre o endereço real do posto. Renomeie para `hub`.
{% endstep %}

{% step %}
### Adicione pontos de interesse

Espalhe pontos de interesse (bairros, garagens) com outros **GIS Points** e faça os motoristas perambularem entre eles com `moveTo(pontoAleatorio)`.
{% endstep %}

{% step %}
### Troque as coordenadas pelo ponto GIS

Em vez de `moveTo(main.hubX, main.hubY)`, use `moveTo(main.hub)`. O AnyLogic calcula a **rota pelas ruas reais** (via serviço de roteamento) e o tempo de deslocamento passa a respeitar a malha viária.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
No GIS, a velocidade em km/h ganha sentido físico direto, e o tempo até o hub reflete distâncias reais. É a forma mais convincente de apresentar um estudo de localização de infraestrutura para um cliente ou órgão público.
{% endhint %}

## Redes e atratores

Para ambientes internos (um pátio, um terminal), use **paths** (caminhos) e **nodes** (áreas). Um **rectangular node** com **attractors** distribui os agentes em posições organizadas (por exemplo, as vagas dos carregadores lado a lado). Assim a animação fica caprichada: cada carro parado numa vaga, e não empilhados num ponto só.
