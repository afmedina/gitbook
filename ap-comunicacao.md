# ap comunicacao

Modelos de verdade quase nunca vivem isolados: os parâmetros vêm de uma planilha e os resultados precisam voltar para uma. O AnyLogic conversa com Excel, arquivos-texto e bancos de dados. Este apêndice segue a mesma linha dos volumes anteriores da série.

## Lendo parâmetros de uma planilha Excel

{% stepper %}
{% step %}
### Adicione o arquivo Excel

Da paleta **Connectivity**, arraste um **Excel File** para o `Main`. Aponte o **File** para a sua planilha (ex.: `frota.xlsx`).
{% endstep %}

{% step %}
### Leia uma célula

Para ler uma célula, use, por exemplo:

```java
int frota = (int) excelFile.getCellNumericValue("Dados", 2, 2); // aba, linha, coluna
nMotoristas = frota;
```
{% endstep %}
{% endstepper %}

Você pode preencher uma tabela inteira de motoristas percorrendo as linhas num laço no **On startup** do `Main`.

## O banco de dados interno do AnyLogic

Todo modelo tem um **banco de dados** embutido (aba **Database** na árvore). Você pode **importar** uma planilha para uma tabela do banco e depois consultá-la com um `query`:

```java
int frota = selectFrom(dados)
              .where(dados.cidade.eq("Sao Paulo"))
              .firstResult(dados.frota);
```

É a forma mais robusta de alimentar modelos grandes: os dados ficam versionados dentro do modelo.

## Gravando resultados em txt

Para registrar os resultados de cada rodada num arquivo de texto (ótimo para pós-processar em Python ou R):

{% stepper %}
{% step %}
### Crie o campo de saída

Crie um **campo** de classe `TextFile` (paleta **Connectivity**) ou use um `PrintWriter` do Java.
{% endstep %}

{% step %}
### Escreva o resultado ao fim da simulação

No **On destroy** do `Main` (ao fim da simulação), escreva a linha de resultado:

```java
traceln(String.format("%d;%d;%.2f", nCarregadores, nMotoristas, nivelServico()));
```
{% endstep %}
{% endstepper %}

O `traceln` joga no console; para arquivo, use um `PrintWriter` aberto no **On startup** e fechado no **On destroy**:

```java
// On startup:
saida = new PrintWriter(new FileWriter("resultados.csv", true));
// On destroy:
saida.printf("%d;%.2f%n", nCarregadores, nivelServico());
saida.close();
```

## Gravando em Excel

Com o **Excel File**, escreva células e salve ao final:

```java
excelFile.setCellValue(nivelServico(), "Saida", 2, 2);
excelFile.writeFile();   // grava em disco
```

{% hint style="success" %}
**Dica:** para varreduras (Parameters Variation), prefira escrever **um CSV por réplica** com uma linha por combinação de parâmetros. Fica trivial abrir tudo depois num só `pandas.read_csv` e montar a curva de dimensionamento fora do AnyLogic, com total controle do gráfico.
{% endhint %}
