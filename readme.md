# README

O **AnyLogic** é um ambiente de simulação que reúne, num único software, os três grandes métodos da simulação: **eventos discretos**, **dinâmica de sistemas** e **modelagem baseada em agentes**. Se você chegou até aqui vindo dos dois primeiros volumes desta série, já conhece bem dois dos três. Falta o mais divertido.

{% hint style="info" %}
Esta é a série completa:

1. [**Tutorial AnyLogic: Modelagem de Processos**](https://tutorial.anylogicbrasil.com.br/) — eventos discretos.
2. [**Tutorial AnyLogic: Dinâmica de Sistemas**](https://tutorial-ds.anylogicbrasil.com.br/) — estoques e fluxos.
3. **Tutorial AnyLogic: Simulação Baseada em Agentes** — este volume.
{% endhint %}

Na **modelagem baseada em agentes** (ou ABM, de _agent-based modeling_) a gente não descreve o sistema de cima para baixo, como um fluxo de processos ou um conjunto de equações. A gente descreve o comportamento de **um** indivíduo — um cliente, um veículo, um vírus, uma pessoa — e deixa o comportamento coletivo _emergir_ da interação de muitos desses indivíduos. O motor central desse comportamento individual é o **statechart** (diagrama de estados), e é nele que vamos passar a maior parte do nosso tempo.

Para não ficar no abstrato, vamos resolver um problema concreto e atualíssimo: **quantos carregadores um hub de recarga de veículos elétricos precisa ter** para atender bem seus motoristas sem desperdiçar dinheiro? É o mesmo tipo de pergunta de dimensionamento de recurso que resolvemos no volume de processos (quantos caixas no banco?), mas agora cada motorista é um agente que dirige pela cidade, fica sem bateria, vai até o hub, enfrenta (ou não) uma fila e volta a rodar.

Ao final deste tutorial você terá:

* construído, do zero, um modelo baseado em agentes com **statechart**, **população de agentes** e **movimento em espaço 2D**;
* coletado indicadores de **nível de serviço**, **fila** e **utilização**;
* enviado o modelo para a **AnyLogic Cloud** e rodado um experimento de **variação de parâmetros** para dimensionar o hub;
* analisado os resultados e tomado uma decisão de engenharia com base neles.

Vamos nessa.

***

_Genoa Soluções —_ [_genoads.com.br_](https://genoads.com.br/)
