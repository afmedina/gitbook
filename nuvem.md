# nuvem

Para determinar o número de carregadores necessário para atingir um nível de serviço adequado, aproveitando que já temos o modelo pronto, basta executá-lo para diferentes valores do parâmetro **`nCarregadores`** e comparar os resultados. A **AnyLogic Cloud** — mesmo na versão pública e gratuita — faz isso rodando as réplicas em paralelo nos servidores, e ainda nos dá uma interface web bonita para mexer nos parâmetros.

{% stepper %}
{% step %}
## Criar a conta

Se ainda não tem, crie uma conta gratuita em [cloud.anylogic.com](https://cloud.anylogic.com/). É a mesma conta do seu login do AnyLogic.
{% endstep %}

{% step %}
## Exportar o modelo para a nuvem

No AnyLogic, com o modelo aberto:

1. Vá em **File > Export > to AnyLogic Cloud** (ou clique no botão da nuvem na barra de ferramentas).
2. Faça login com sua conta, se solicitado.
3. Dê um nome ao modelo (por exemplo, `Hub de Recarga VE`) e confirme o envio.

O AnyLogic empacota o modelo e o carrega para a sua conta na nuvem. Ao terminar, ele abre o modelo no navegador.

{% hint style="info" %}
Na versão **pública** da AnyLogic Cloud, os modelos que você sobe ficam **visíveis publicamente** (é o preço da gratuidade). Não suba modelos confidenciais de cliente para a nuvem pública — para isso existe a Private Cloud. Para um exercício didático como este, está perfeito.
{% endhint %}
{% endstep %}

{% step %}
## Conhecer a interface da nuvem

No navegador, você verá o seu modelo com um painel de **inputs** (os parâmetros do experimento) à esquerda e a **animação/resultados** à direita. Você pode rodar o modelo ali mesmo, mudar `nCarregadores` e ver o efeito — uma forma excelente de deixar um cliente "brincar" com o modelo sem instalar nada.

Mas o que queremos é mais poderoso: um **experimento** que varra automaticamente todos os valores de `nCarregadores`. Vamos criá-lo na próxima página — e você pode criá-lo tanto no AnyLogic (desktop) quanto direto na nuvem. Vamos pelo caminho do desktop, que é o mais didático.
{% endstep %}
{% endstepper %}
