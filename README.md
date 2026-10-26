# Olist: E-Commerce Public Dataset 2017/2018

Exploração do dataset público de pedidos da Olist Store, recortado no período completo de **janeiro de 2017 a agosto de 2018**.

**Stack:** Python · Jupyter · CSV

## O que o projeto mostra

- **Base real de marketplace:** pedidos, itens, pagamentos, avaliações, clientes, vendedores, produtos e geolocalização, ligados por chaves (`order_id`, `customer_id`, `product_id`, `seller_id`).
- **Recorte temporal validado:** a base original vai de set/2016 a out/2018, mas só jan/2017 a ago/2018 tem volume contínuo. Os demais meses foram removidos (ver [Recorte de dados](#recorte-de-dados)).
- **Integridade referencial mantida:** nenhum item, pagamento ou avaliação aponta para um pedido que não existe mais.

## Descrição original (Kaggle)

Texto reproduzido de [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), publicado por Olist no Kaggle sob a licença CC BY-NC-SA 4.0.

### Brazilian E-Commerce Public Dataset by Olist

Welcome! This is a Brazilian ecommerce public dataset of orders made at [Olist Store](http://www.olist.com). The dataset has information of 100k orders from 2016 to 2018 made at multiple marketplaces in Brazil. Its features allows viewing an order from multiple dimensions: from order status, price, payment and freight performance to customer location, product attributes and finally reviews written by customers. We also released a geolocation dataset that relates Brazilian zip codes to lat/lng coordinates.

This is real commercial data, it has been anonymised, and references to the companies and partners in the review text have been replaced with the names of Game of Thrones great houses.

### Join it With the Marketing Funnel by Olist

We have also released a [Marketing Funnel Dataset](https://www.kaggle.com/olistbr/marketing-funnel-olist/home). You may join both datasets and see an order from Marketing perspective now!

**Instructions on joining are available on this [Kernel](https://www.kaggle.com/andresionek/joining-marketing-funnel-with-brazilian-e-commerce).**

### Context

This dataset was generously provided by Olist, the largest department store in Brazilian marketplaces. Olist connects small businesses from all over Brazil to channels without hassle and with a single contract. Those merchants are able to sell their products through the Olist Store and ship them directly to the customers using Olist logistics partners. See more on our website: [www.olist.com](https://www.olist.com)

After a customer purchases the product from Olist Store a seller gets notified to fulfill that order. Once the customer receives the product, or the estimated delivery date is due, the customer gets a satisfaction survey by email where he can give a note for the purchase experience and write down some comments.

#### Attention

1. An order might have multiple items.
2. Each item might be fulfilled by a distinct seller.
3. All text identifying stores and partners where replaced by the names of Game of Thrones great houses.

#### Example of a product listing on a marketplace

![Example of a product listing on a marketplace](https://i.imgur.com/JuJMns1.png)

### Data Schema

The data is divided in multiple datasets for better understanding and organization. Please refer to the following data schema when working with it:

![Data Schema](https://i.imgur.com/HRhd2Y0.png)

### Classified Dataset

We had previously released a classified dataset, but we removed it at *Version 6*. We intend to release it again as a new dataset with a new data schema. While we don't finish it, you may use the classified dataset available at the *Version 5* or previous.

### Inspiration

Here are some inspiration for possible outcomes from this dataset.

**NLP:**   

This dataset offers a supreme environment to parse out the reviews text through its multiple dimensions.

**Clustering:**  

Some customers didn't write a review. But why are they happy or mad?

**Sales Prediction:**  

With purchase date information you'll be able to predict future sales.

**Delivery Performance:**  

You will also be able to work through delivery performance and find ways to optimize delivery times.

**Product Quality:**   

Enjoy yourself discovering the products categories that are more prone to customer insatisfaction.

**Feature Engineering:**   

Create features from this rich dataset or attach some external public information to it.

### Acknowledgements

Thanks to Olist for releasing this dataset.

## Recorte de dados

A base original (versão de 2021-10-01 no Kaggle) tem pedidos de 2016-09-04 a 2018-10-17, mas os extremos são incompletos: 2016 tem só 329 pedidos (nenhum em novembro), e set e out/2018 têm 20 pedidos, quase todos cancelados. Por isso o repositório mantém **apenas pedidos com** `order_purchase_timestamp` **entre 2017-01-01 e 2018-08-31**.

O filtro foi aplicado em cascata, sem alterar o formato nem o conteúdo das linhas mantidas:

| Tabela | Regra | Linhas |
|---|---|---|
| `olist_orders_dataset.csv` | data da compra no período | 99.092 (de 99.441) |
| `olist_order_items_dataset.csv` | `order_id` presente em orders | 112.279 (de 112.650) |
| `olist_order_payments_dataset.csv` | `order_id` presente em orders | 103.520 (de 103.886) |
| `olist_order_reviews_dataset.csv` | `order_id` presente em orders | 98.879 (de 99.224) |
| `olist_customers_dataset.csv` | `customer_id` presente em orders | 99.092 (de 99.441) |
| `olist_products_dataset.csv` | sem filtro | 32.951 |
| `olist_sellers_dataset.csv` | sem filtro | 3.095 |
| `olist_geolocation_dataset.csv` | sem filtro | 1.000.163 |
| `product_category_name_translation.csv` | sem filtro | 71 |

Pontos de atenção:

- **Pedidos sem itens:** 739 pedidos do período não têm linha em `order_items` (em geral cancelados ou indisponíveis).
- `shipping_limit_date` **fora do período:** a coluna chega a 2020-04-09 em `order_items`, mesmo em pedidos do recorte. É um valor suspeito do dado original e foi mantido.
- **Pedidos em andamento:** pedidos de agosto/2018 ainda aparecem como `shipped` ou `invoiced`, porque o status era o do momento da extração.
- **Tabelas de dimensão:** produtos, vendedores e geolocalização não foram filtrados, então podem conter registros sem pedido no período.

## Tabelas

| Arquivo | Conteúdo |
|---|---|
| `olist_orders_dataset.csv` | Pedidos, status e datas de compra, aprovação, envio, entrega e entrega estimada |
| `olist_order_items_dataset.csv` | Itens de cada pedido, com produto, vendedor, preço e frete |
| `olist_order_payments_dataset.csv` | Formas e valores de pagamento de cada pedido |
| `olist_order_reviews_dataset.csv` | Nota e comentário do cliente sobre o pedido |
| `olist_customers_dataset.csv` | Clientes e localização |
| `olist_sellers_dataset.csv` | Vendedores e localização |
| `olist_products_dataset.csv` | Produtos, categoria e dimensões |
| `olist_geolocation_dataset.csv` | CEPs (prefixo) com latitude e longitude |
| `product_category_name_translation.csv` | Nome das categorias em português e inglês |

## Como usar

```bash
git clone https://github.com/gustamdev/Olist-E-Commerce-Public-Dataset-2017-2018.git
cd Olist-E-Commerce-Public-Dataset-2017-2018
jupyter notebook exploration.ipynb
```

## Licença e créditos

Dados: [Olist](https://www.olist.com), publicados no [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) sob a licença [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). Este repositório traz uma versão recortada desses dados e mantém a mesma licença.
