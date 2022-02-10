Bem vinda(o) ao nosso challenge para pessoas de engenharia. 
Antes de iniciar, se for publicar o código em algum lugar, **não utilize o nome da Estoca em nenhuma parte**.

Feedbacks são super importantes e bem vindos!!! Fique a vontade de enviar sugestões no **tech@estoca.com.br**

# Considerações sobre o desafio
* O teste deve ser realizada por somente uma pessoa;
* No dia marcado para a entrevista técnica haverá uma discussão sobre as decisões e questões técnicas baseadas no seu código;
* Não estamos buscando uma solução absolutamente perfeita, buscamos uma solução de acordo com seu nível e funcional (se seu nível é de SR, esperamos ver uma solução com nível de SR);
* Limite de tempo: 1 semana;
* Após finalizar o teste envie o link do repositório para **tech@estoca.com.br**

# Tecnologias
Se possível, as seguintes tecnologias devem ser utilizadas para resolver esse desafio. Entendemos que é possível que você não tenha muita experiência com essas tecnologias, mas para nós é muito importante entender a capacidade de um engenheiro de aprender.

* Python / Django
* Javascript / NodeJS
* C# (preferêncialmente .NET 6 ou .NET Core)

# O que será avaliado
* Design de código, práticas e padrões;
* Conhecimento da linguagem e de orientação a objetos;
* Utilização correta de git;
* Comentários, documentação das decisões e como rodar o projeto no readme do projeto;
* Construção de testes;

# Considerações técnicas 
* Se você for utilizar novas estruturas para armazenar/recuperar informações nos cenários abaixo, apenas descreva no código a forma dessas estruturas (e.g. nova tabela no banco que tenha os campos X, Y e Z), não é necessário desenvolver um código com a criação das estruturas. O que queremos entender é como você irá usar esses recursos.
* Se necessário, pode considerar que existe um ORM que busca e grava informações de eficientemente

# Desafios

## 1. Integração de pedidos

Você precisa integrar pedidos de um sistema ERP onde a API te envia um JSON [com o padrão que está aqui](#retorno-api-de-pedidos)
. Esse sistema não tem uma funcionalidade de webhook em que você é notificado cada vez que um novo pedido é criado. Considerando que você consulta a API a cada 5 minutos, desenvolva uma função que salva novos pedidos.

### Considerações
Output esperado:
ID dos pedidos criados

#### As seguintes funções já existem 
**save_order_to_db(_order)** 
* Recebe a estrutura [que está aqui](#retorno-pedido-get-orders)
* Salva as informações nas tabelas do banco de dados
* Retorna o ID do pedido criado

**get_orders(_filters)** 
* Recebe qualquer campo da estrutura [que está aqui](#retorno-pedido-get-orders) como filtro. E.g. created_at=2021-04-01
* Faz a consulta no banco de dados de acordo com os filtros
* Retorna array dos pedidos com a estrutura [que está aqui](#retorno-pedido-get-orders)

## 2. Controle de Estoque

Para um controle efetivo do estoque, para cada novo pedido criado deve ser feito uma reserva de estoque antes da separação do pedido. Considerando que a entrada do estoque é realizada por um registro único por cada item em uma tabela **inventory_entry** (i.e. cada registro representa uma entrada), desenvolva duas funções **(1)** que verifica se existe estoque disponível e **(2)** outra que realiza a reserva do estoque. 

### Considerações
Input esperado:
Pedido com a estrutura [que está aqui](#retorno-api-de-pedidos)

Output esperado:
ID dos pedidos criados

#### As seguintes funções já existem
**get_inventory_entries(_sku)** 
* Recebe o sku (código único) de um item de um pedido
* Retorna as entradas deste item no formato [que está aqui](#retorno-inventory)

## 3. Análise de "Sem Estoque"

Para controle de estoque acurado é preciso entender bem quais itens comprar com antecedência para que não ocorra quebra de estoque mas que também não se utilize muito tempo de estoque. 
Desenvolva uma função que estime quanto tempo um determinado SKU vai durar dentro do estoque. 

### Considerações
Input esperado:
Pedido com a estrutura [que está aqui](#retorno-api-de-pedidos)

Output esperado:
Dicionário de skus com datas esperadas esperadas para finalização

# Payloads

## Retorno API de pedidos

```json
{
    “Id”: 12,
    “Date”: 2021-01-01T10:23:23,
    “Total Value”: 12.5,
    “Status”: 3,
    “Channel”: “Marketplace”,
    “Customer”:{
        “Name”: “John Junior”,
        “Document”: “182731982”,
        “Address”: “Junior St. 78”,
        “Number”: “901”,
        “Complement”: “Apto 88”,
        “Neighborhood”: “Central Park”,
        “City”: “Big City”,
        “State”: “ST”,
    },
    “Invoice”:{
        “Number”: “1232”,
        “Serie”: “1”,
        “Access Key”: “35210411281358000156550010000361091656273380”,
        “Issue Date”: “2021-01-02T11:23:23”,
        “Value”: “13.4”,
    },
    “Delivery”:[{
        “Transporter”: “Express Delivery”,
        “Tracking number”: “AB312313CA1”,
        “Address”: “Junior St. 78”,
        “Number”: “901”,
        “Complement”: “Apto 88”,
        “Neighborhood”: “Central Park”,
        “City”: “Big City”,
        “State”: “ST”,
    }],
    “Items”: [{
        “Id”: “12_1”,
        “Sku”: “PRODUCT_1”,
        “Name”: “Awesome Super Product”,
        “Quantity”: “2.0”,
        “Price”: “2.5”,
    }]
}
```

## Retorno Pedido Get Orders

```json
[
    {
        “id”: 21b1c6e4-9d7f-4f78-8e82-19491a0f5a12,
        “external_id”: “32”,
        “store_id”: 208a4cef-2b7b-4790-b369-e5636cdf466a,
        “created_at”: 2021-01-01T10:23:23,
        “is_picked”: false,
        “is_stockout”: false
    }
]
```

## Retorno Inventory

```json
[
    {
        “id”: 21b1c6e4-9d7f-4f78-8e82-19491a0f5a12,
        “product_id”: 0af09adc-139c-11eb-9921-38f9d361ecb6,
        “sku”: “PRODUCT_1”,
        “created_at”: 2021-01-01T10:23:23,
        “quantity”: 1,
    }
]
```
