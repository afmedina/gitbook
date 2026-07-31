# desafio

Antes de fechar o modelo, um desafio para verificar se você realmente entendeu o que está acontecendo lá dentro. Pense antes de rodar — a graça está em **prever** e depois **conferir**.

> **Desafio:** imagine que o hub tivesse um número **enorme** de carregadores (digamos, `nCarregadores = 100`, muito mais que a frota inteira). Nesse cenário, para quanto tende o **tempo médio que um motorista passa no hub** (fila + recarga)? E o **nível de serviço**? Explique _por quê_, sem apelar para a simulação.
>
> **Bônus:** se você **dobrar** a frota (`nMotoristas` de 25 para 50), o número de carregadores necessário para manter 90% de serviço também **dobra**? Ou cresce mais devagar? Por quê?

Tente responder com suas palavras. A resposta está na última página dos apêndices, em [Resposta do desafio](/broken/pages/f6a66f4e9a8f3c80216360d0e4d99b85ebeb109d).

{% hint style="success" %}
**Dica:** para o bônus, pense no que o hub tem de "compartilhado". Quando muitos motoristas dividem o **mesmo** conjunto de carregadores, os picos de demanda de uns preenchem as folgas de outros. Esse fenômeno tem nome na Teoria das Filas.
{% endhint %}
