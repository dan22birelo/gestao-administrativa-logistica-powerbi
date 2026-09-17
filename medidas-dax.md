# Medidas DAX utilizadas no projeto

Este documento apresenta as principais medidas DAX utilizadas no painel **Gestão Digital de Processos Administrativos e Logísticos**.

Os cálculos foram desenvolvidos no Microsoft Power BI utilizando principalmente as tabelas `BI_Pedidos`, `BI_Estoque` e `Pedidos`.

## Indicadores de pedidos

### Total de Pedidos

Realiza a contagem distinta dos pedidos cadastrados na base.

```DAX
Total de Pedidos =
DISTINCTCOUNT(BI_Pedidos[ID_Pedido])
```

### Valor Líquido

Calcula a soma dos valores líquidos dos pedidos, considerando os descontos aplicados.

```DAX
Valor Líquido =
SUM(BI_Pedidos[Valor_Liquido])
```

### Pedidos Entregues

Conta somente os pedidos que possuem status igual a “Entregue”.

```DAX
Pedidos Entregues =
CALCULATE(
    DISTINCTCOUNT(BI_Pedidos[ID_Pedido]),
    BI_Pedidos[Status] = "Entregue"
)
```

### Pedidos Atrasados

Conta os pedidos que foram entregues com atraso e os pedidos que ainda se encontram atrasados.

```DAX
Pedidos Atrasados =
CALCULATE(
    DISTINCTCOUNT(BI_Pedidos[ID_Pedido]),
    BI_Pedidos[Situacao_Prazo] IN {
        "Atrasado",
        "Entregue com atraso"
    }
)
```

### Entregas no Prazo

Calcula a proporção de pedidos entregues no prazo em relação ao total de pedidos entregues.

```DAX
Entregas no Prazo =
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT(BI_Pedidos[ID_Pedido]),
        BI_Pedidos[Situacao_Prazo] = "Entregue no prazo"
    ),
    CALCULATE(
        DISTINCTCOUNT(BI_Pedidos[ID_Pedido]),
        BI_Pedidos[Status] = "Entregue"
    ),
    0
)
```

A medida foi formatada como porcentagem no Microsoft Power BI.

### Margem Estimada

Calcula a soma da margem estimada dos pedidos.

```DAX
Margem Estimada =
SUM(BI_Pedidos[Margem_Estimada])
```

## Indicadores de estoque

### Estoque Atual

Calcula a quantidade total disponível em estoque.

```DAX
Estoque Atual =
SUM(BI_Estoque[Estoque_Atual])
```

### Estoque Mínimo

Calcula a soma dos níveis mínimos definidos para os produtos.

```DAX
Estoque Mínimo =
SUM(BI_Estoque[Estoque_Minimo])
```

### Produtos Abaixo do Mínimo

Conta quantos produtos possuem estoque atual inferior ao estoque mínimo.

```DAX
Produtos Abaixo do Mínimo =
CALCULATE(
    DISTINCTCOUNT(BI_Estoque[ID_Produto]),
    BI_Estoque[Situacao_Estoque] = "Abaixo do mínimo"
) + 0
```

O acréscimo de `+ 0` faz com que o cartão apresente o número zero quando nenhum produto estiver abaixo do estoque mínimo.

## Indicadores de clientes

### Clientes com Pedidos

Conta de forma distinta os clientes que possuem pelo menos um pedido na base.

```DAX
Clientes com Pedidos =
DISTINCTCOUNT(BI_Pedidos[ID_Cliente])
```

### Cidades Atendidas

Conta a quantidade distinta de cidades que possuem clientes com pedidos.

```DAX
Cidades Atendidas =
DISTINCTCOUNT(Pedidos[Cidade])
```

## Campos calculados na base

Além das medidas DAX, a solução utiliza campos preparados no Microsoft Excel:

- Dias de prazo;
- Dias de atraso;
- Situação do prazo;
- Custo total estimado;
- Margem estimada;
- Situação do estoque;
- Valor bruto;
- Valor líquido.

Esses campos foram importados para o Microsoft Power BI e utilizados na construção dos indicadores, filtros e visualizações.

## Resultados apresentados pelas medidas

As medidas contribuíram para apresentar os seguintes resultados:

- 120 pedidos analisados;
- 87 pedidos entregues;
- 75 pedidos atrasados;
- 43,7% de entregas no prazo;
- Valor líquido total de R$ 361.319,02;
- Margem estimada de R$ 87.835,30;
- Estoque atual de 563 unidades;
- Estoque mínimo de 322 unidades;
- Nenhum produto abaixo do estoque mínimo;
- 28 clientes com pedidos;
- Nove cidades atendidas.

## Observação

As medidas foram desenvolvidas para uma base acadêmica fictícia. Os resultados demonstram o funcionamento técnico do protótipo e não representam dados reais ou confidenciais da empresa.
