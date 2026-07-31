# rodando analise

Você rodou o modelo com `nCarregadores = 3` e anotou o nível de serviço. Rode-o **de novo**, sem mudar nada. Reparou que o número mudou um pouco?

Isso acontece porque o modelo é **estocástico**: as autonomias e os tempos de recarga são sorteados de distribuições, e a cada execução os sorteios são diferentes. Uma única rodada é apenas **uma** realização possível do futuro. Tomar uma decisão de investimento (quantos carregadores comprar) com base em uma rodada é como decidir se um dado é viciado jogando-o uma única vez.

{% hint style="info" %}
Se você quer que uma execução seja idêntica à anterior (para depurar), fixe a **semente** do gerador de números aleatórios nas propriedades do experimento **Simulation → Randomness → Fixed seed**. Para avaliar desempenho, porém, queremos o contrário: **muitas** sementes diferentes.
{% endhint %}

## O que precisamos de verdade

Para decidir com segurança, precisamos responder duas perguntas de uma vez:

1. Para **cada** valor candidato de `nCarregadores` (1, 2, 3, ... 8), qual é o nível de serviço **médio**?
2. Qual o **menor** `nCarregadores` cujo nível de serviço médio fica **≥ 90%**?

Fazer isso na mão seria: mudar o parâmetro, rodar N réplicas, anotar a média, repetir para o próximo valor. Oito valores × 30 réplicas = 240 execuções. Ninguém merece.

É exatamente para isso que existe o experimento **Parameters Variation** — e, melhor ainda, podemos deixá-lo rodando na **nuvem**, em paralelo, enquanto tomamos um café. Vamos lá.
