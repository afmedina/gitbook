# resposta desafio

## O tempo no hub e o nível de serviço com carregadores "infinitos"

Com um número enorme de carregadores, **nunca** falta vaga. Todo motorista que chega é atendido na hora: a espera na fila tende a **zero**. Logo:

* O **tempo médio no hub** tende ao **tempo médio de recarga** — no nosso modelo, a média da distribuição `triangular(30, 45, 75)`, ou seja, **50 minutos** (a média de uma triangular é (min+moda+max)/3 = (30+45+75)/3 = 50). Não há mais nada além da recarga em si consumindo tempo dentro do hub.
* O **nível de serviço** tende a **100%**, porque toda recarga começa com espera zero — bem abaixo do limite de 10 minutos.

A lição é a mesma do desafio do volume de Modelagem de Processos (onde o tempo de permanência tendia ao tempo de serviço): **adicionar recurso só remove o tempo de espera; nunca remove o tempo de serviço**. Existe um piso físico — o tempo de recarga — que nenhum investimento em carregadores derruba. É por isso que a curva de nível de serviço **satura**: uma vez que a espera já é praticamente zero, não há mais o que melhorar comprando carregadores.

## Bônus: dobrar a frota **não** dobra os carregadores

Ao dobrar a frota, a demanda por recarga (a "carga oferecida") aproximadamente dobra. A intuição diz que os carregadores também deveriam dobrar. Mas eles crescem **um pouco mais devagar** — o hub grande é mais **eficiente por carregador** que o pequeno.

O motivo é o **compartilhamento de recurso** (_pooling_), um resultado clássico da Teoria das Filas (o comportamento dos sistemas M/M/c). Quando muitos motoristas dividem o mesmo conjunto de carregadores, o pico de demanda de um cliente encontra, com frequência, a folga deixada por outro. Quanto maior o pool, menor a chance de **todos** os carregadores estarem ocupados ao mesmo tempo. Em números redondos: um hub para 25 carros talvez precise de 4 carregadores (0,16 por carro); um hub para 50 talvez precise de 7 (0,14 por carro), não 8.

{% hint style="info" %}
Esse é um argumento poderoso a favor de **centralizar** a infraestrutura de recarga: dois hubs de 4 carregadores atendem _pior_ que um único hub de 8, para a mesma frota total, justamente porque perdem o efeito de pooling. Vale a pena testar isso no seu modelo — é uma ótima extensão.
{% endhint %}

E o melhor: você **não precisa** confiar apenas na intuição. Rode o `Parameters Variation` variando também `nMotoristas` e confira a curva. O modelo que você construiu já responde a essa pergunta.
