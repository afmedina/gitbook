# ap statecharts

O statechart do nosso motorista usou o essencial. O AnyLogic oferece bastante mais. Este apêndice é um mapa dos recursos que você vai querer conforme seus modelos crescem.

## Os cinco gatilhos de transição

Toda transição dispara por **um** destes motivos (propriedade **Triggered by**):

| Gatilho           | Dispara quando…                        | Usamos em                                   |
| ----------------- | -------------------------------------- | ------------------------------------------- |
| **Timeout**       | passa um tempo definido                | `Rodando→IndoAoPosto`, `Carregando→Rodando` |
| **Rate**          | segundo uma taxa (tempo exponencial)   | contágios, chegadas                         |
| **Condition**     | uma expressão booleana fica verdadeira | — (cuidado, veja abaixo)                    |
| **Message**       | chega uma mensagem ao agente           | `NaFila→Carregando`                         |
| **Agent arrival** | o agente termina um `moveTo`           | `IndoAoPosto→NaFila`                        |

### A armadilha da transição por condição

Uma transição por **Condition** só é reavaliada quando **algo muda no próprio agente** (um evento dele, ou uma chamada explícita a `onChange()`). Ela **não** percebe mudanças em outros agentes ou no `Main`. Foi por isso que **não** usamos `carregadoresLivres > 0` como condição: quem libera o carregador é outro motorista, e os que esperam nunca seriam notificados.

Se você quiser mesmo usar condição, o padrão correto é **forçar a reavaliação**: quando o `Main` muda, chame `onChange()` nos agentes interessados — por exemplo, na função `liberarCarregador()`, fazer `for (Motorista m : motoristas) m.onChange();`. Funciona, mas percorre a população inteira a cada liberação. A abordagem por **mensagem** que adotamos é mais direta e escala melhor: avisa **só** quem precisa.

## Guardas (guard)

Além do gatilho, uma transição pode ter uma **Guard** — uma condição extra que precisa ser verdadeira para a transição valer. Útil para desempatar quando duas transições saem do mesmo estado. Ex.: dois `Message` saindo de `NaFila`, um com guarda `msg.equals("vaga")` e outro com `msg.equals("cancelar")`.

## Estados compostos

Um **State** pode conter outros estados (basta desenhá-los dentro dele). Isso permite hierarquia: por exemplo, um estado `EmOperacao` contendo `Rodando`, `IndoAoPosto`, `NaFila` e `Carregando`, ao lado de um estado `Manutencao` fora dele. Uma transição saindo da **borda** de `EmOperacao` vale para _qualquer_ subestado — ótimo para modelar uma pane que pode acontecer a qualquer momento ("de qualquer estado interno, se quebrar, vá para `Manutencao`").

## Pseudo-estados: inicial, branch e história

* **Initial state pointer:** o ponto de entrada de um estado composto (qual subestado começa ativo).
* **Branch (junção):** um losango que ramifica o fluxo por condições — como um `if/else` visual. Útil logo após uma transição para decidir o próximo estado.
* **History (H):** faz um estado composto "lembrar" em qual subestado estava quando saiu, e voltar para ele. Ex.: ao sair da `Manutencao`, retomar exatamente de onde parou.

## Transições internas × externas

A **transição interna** que usamos em `Rodando` (para reperambular) **não** reinicia o estado: as ações de entrada/saída de `Rodando` não são reexecutadas. Uma transição externa (que sai e volta ao mesmo estado) **reexecutaria** a entrada. Escolha conforme queira ou não "resetar" o estado.

## Funções úteis do statechart

* `statechart.getState()` — o estado simples ativo agora.
* `inState(NomeDoEstado)` — testa se um estado (ou composto que o contém) está ativo.
* `statechart.receiveMessage(msg)` / `send(msg, agente)` — comunicação.
* `statechart.fireEvent("nome")` — dispara transições configuradas como _"On receiving a specific event"_.
