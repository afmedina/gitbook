# criando modelo

Abra o AnyLogic e crie um novo modelo: **File > New > Model**. Dê o nome de `HubRecargaVE` e, muito importante, defina a **unidade de tempo do modelo** como **minutes** (minutos). Clique em **Finish**.

{% hint style="info" %}
A unidade de tempo é uma decisão de projeto. Como as autonomias e os tempos de recarga do nosso problema estão na casa de dezenas a centenas de minutos, **minutos** é a escala mais confortável. Se você escolher a unidade errada agora, vai penar depois convertendo tudo.
{% endhint %}

## O agente `Main` e o agente `Motorista`

Todo modelo do AnyLogic começa com um agente chamado **Main**. Pense no `Main` como o "mundo": é ele que vai conter o **espaço** onde os veículos circulam, a **população** de motoristas e o **hub** de recarga. O `Main` é o palco; os motoristas são os atores.

Precisamos, então, de um segundo tipo de agente para representar cada motorista. Vamos criá-lo já como uma **população** dentro do `Main`.

{% stepper %}
{% step %}
## Adicione um agente ao `Main`

Na paleta **Agent** (à esquerda), arraste **Agent** para dentro do diagrama do `Main`. O assistente **New agent** vai abrir.
{% endstep %}

{% step %}
## Crie uma população de agentes

Escolha **Population of agents** (queremos vários motoristas, não um só) e clique em **Next**.
{% endstep %}

{% step %}
## Defina o tipo e o nome da população

Em **Agent type name** digite `Motorista`; em **Agent population name** o AnyLogic sugere `motoristas`. Aceite. **Next**.
{% endstep %}

{% step %}
## Escolha a animação

Na tela de animação, escolha uma figura 2D — por exemplo **Car** (carro). **Next**.
{% endstep %}

{% step %}
## Pule os parâmetros

Ele pergunta se você quer adicionar parâmetros. Pode pular por enquanto (**Next** / **Finish**).
{% endstep %}

{% step %}
## Defina o tamanho inicial da população

Quando perguntar o **tamanho inicial da população**, deixe **0** por enquanto — vamos controlá-lo por um parâmetro. **Finish**.
{% endstep %}
{% endstepper %}

Pronto: você acabou de criar o tipo de agente `Motorista`, a população `motoristas` dentro do `Main`, e o AnyLogic abriu o diagrama do `Motorista` numa nova aba. Repare que agora você tem **duas abas de agente**: `Main` e `Motorista`.

{% hint style="success" %}
**Dica:** dê um clique duplo em qualquer agente na árvore do projeto (canto superior esquerdo) para abrir seu diagrama. Você vai alternar bastante entre `Main` e `Motorista` neste tutorial.
{% endhint %}

Nas próximas páginas vamos, nesta ordem: dar **parâmetros e variáveis** ao `Motorista`, desenhar o seu **statechart**, montar o **espaço 2D** e o **hub** no `Main`, e por fim ligar a **lógica de fila** dos carregadores.
