# premissa

Este tutorial parte da premissa de que você **já teve contato com simulação** — de preferência, já passou pelos volumes de Modelagem de Processos e de Dinâmica de Sistemas, ou tem experiência equivalente. Em nenhum momento vamos explicar o que é um modelo, o que é o tempo de simulação, o que é uma variável aleatória ou por que rodamos várias réplicas. Assumimos que você já entende o básico.

O que **é** novo aqui é a forma de pensar. Se na modelagem de processos você pensava em _entidades fluindo por blocos_, e na dinâmica de sistemas você pensava em _estoques e taxas_, na modelagem baseada em agentes você vai pensar em **indivíduos com comportamento próprio**. A pergunta que você faz muda de “por onde a entidade passa?” para “o que este agente faz, e como ele reage ao que acontece à sua volta?”.

{% hint style="success" %}
**Dica:** se você domina statecharts, metade do trabalho de ABM no AnyLogic já está feita. Vale a pena ler com calma a Parte II.
{% endhint %}

Também assumimos que você tem o **AnyLogic PLE (Personal Learning Edition)** instalado, pacote específico para auto aprendizagem ou uso em sala de aula. Se ainda não o fez, você deve baixar a versão PLE por meio deste [link](https://www.anylogic.com/downloads/) (ao final da página do link, você encontra os requisitos mínimos de hardware e software para executar o AnyLogic).

A instalação para **Windows, Mac ou Linux** é feita sem percalços, seguindo as instruções que aparecem na tela.

<figure><img src="https://tutorial.anylogicbrasil.com.br/~gitbook/image?url=https%3A%2F%2F2363056371-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MF_N8zy2LrIEjFRHA0F%252F-MSKom79u32A9JuIos3E%252F-MSKp8h-Kiznu7VU4ysu%252Fimage.png%3Falt%3Dmedia%26token%3Dede1b764-0f52-4e05-873b-7bf7c3dbe5b4&#x26;width=400&#x26;dpr=3&#x26;quality=100&#x26;sign=b1287b90&#x26;sv=2" alt=""><figcaption></figcaption></figure>
