O SQL é usado para consultar os dados e salvar o resultado em um arquivo `.csv`. Em seguida, o Excel pode auxilar na visualização dos dados e tomada de decisão.

---
# Configurações
********************
Baixar e instalar o pgAdmin: Ferramenta gráfica para a administração do PostgreSQL e o próprio PostgreSQL: Sistema gerenciador do banco de dados (SGBD) que possibilita a consulta aos dados com o uso da linguagem SQL.

```
https://www.postgresql.org/download/
```

---
# Consultas

**Antes de iniciar é importante visualizar o nome das tabelas e suas colunas para não cometer erros durante as consultas.**

```sql
select coluna
from schema.tabela
```

O **schema** Sales (Vendas) possui uma ou mais tabelas; Tabelas existentes dentro de Sales:
`customers`, `funnel`, `products` e `stores`.

Diagrama de relacionamento entre as tabelas:

![Diagrama](Diagrama.pdf)

## SELECT

`Select` é um comando usado para selecionar colunas de tabelas.

```sql
select coluna
from schema.tabela
```

### Exemplos

Listar os e-mails dos clientes da tabela `sales.customers`.

```sql
select email
from sales.customers
```

Listar os e-mail e o nome dos clientes da tabela `sales.customers`.

```sql
select email, first_name
from sales.customers
```

Listar todas as informações da tabela `sales.customers`.

```sql
select *
from sales.customers
```

### Resumo 

1. Quando selecionar mais de uma coluna, elas devem ser separadas por vírgula.
2. Evitar vírgula antes do comando `from`.
3. Uso do `*` para selecionar todas as colunas da tabela.

## DISTINCT

`Distinct` é um comando usado para remover linhas duplicadas e mostrar apenas linhas distintas; Muito usado na etapa de exploração dos dados.

```sql
select distinct coluna
from schema.tabela
```

### Exemplos

Listar as marcas de carros que constam na tabela `sales.products`.

```sql
select brand
from sales.products
```

Listar as marcas de carros removendo os valores dulicados que constam na tabela `sales.products`.

```sql
select distinct brand
from sales.products
```

Listar as marcas e anos de modelo removendo os valores dulicados que constam na tabela `sales.products`.

```sql
select distinct brand, model_year
from sales.products
```

### Resumo

1.  Muito usado na etapa de exploração dos dados.
2. Ao selecionar mais de uma coluna, o comando `select distinct` irá retornar todas as combinações distintas.

## WHERE

`where` é usado para filtrar as linhas da tabela de acordo com a condição.

```sql
select coluna1, coluna2, coluna3
from schema.tabela
where condição
```

### Exemplos

Listar todos os estados removendo os valores dulicados que constam na tabela `sales.costumers`

```sql
select distinct state
from sales.customers
```

Listar todos os e-mails dos clientes que moram no estado de Santa Catarina na tabela `sales.costumers`.

```sql
select email, state
from sales.customers
where state = 'SC'
```

Listar os e-mails dos clientes que moram no estado de Santa Catarina ou Mato Grosso do Sul na tabela `sales.costumers`.

```sql
select email, state
from sales.customers
where state = 'SC' or state = 'MS'
```

**A empresa percebeu que os clientes com mais de 30 anos são mais propensos a comprar um novo veículo**. Com isso, você deve listar os e-mails dos clientes que moram no estado de Santa Catarina ou Mato Grosso do Sul e que tenha mais de 30 anos.

```sql
select email, state, birth_date
from sales.customers
where (state = 'SC' or state = 'MS') and birth_date <= '1995-06-12' 

--- '1995-06-12' ou '19950612', é opcional!


-- Data atual: 2025-06-12
-- 2025 - 30 (Ano atual - idade) = 1995
-- '1995-06-12'
```

### Resumo

1. No PostgreSQL é usado aspas simples para delimitar strings.
2. É possível combinar mais de uma condição usando os operados lógicos.
3. No PostgreSQL as datas são escritas no formato 'YYYY-MM-DD' ou 'YYYYMMDD'.

## ORDER BY

`order by` é usado para ordenar a seleção de acordo com uma regra defininda.

```sql
select coluna1, coluna2, coluna3
from schema.tabela
where condição
order by coluna1
```

Listar todas as informações da tabela `sales.products`.

```sql
select *
from sales.products
```

Listar produtos da tabela `sales.products` na ordem crescente com base no preço.

```sql
select *
from sales.products
order by price
```

Listar produtos da tabela `sales.products` na ordem decrescente com base no preço.

```sql
select *
from sales.products
order by price desc
```

Listar os estados removendo os valores dulicados que constam na tabela `sales.customers`.

```sql
select distinct state
from sales.customers
```

Listar os estados na ordem crescente e removendo os valores dulicados que constam na tabela `sales.customers`.

```sql
select distinct state
from sales.customers
order by state
```

### Resumo

1. Por padrão o comando ordena na ordem crescente.
2. Comando `desc` para ordem decrescente.
3. Strings são ordenadas de forma alfabética.

## LIMIT

`limit` é usado para limitar  o Nº de linhas da consulta. Muito usado na etapa de exploração dos dados.

```sql
select coluna1, coluna2, coluna3
from schema.tabela
where condição
order by coluna1
limit 1
```

Listar as 10 primeiras linhas da tabela `sales.funnel`

```sql
select *
from sales.funnel
limit 10
```

Listar os 10 produtos mais caros da tabela `sales.products`.

```sql
select *
from sales.products
order by price desc
limit 10
```

### Resumo

1. O `limit` deve ser o último comando da consulta.
2. Muito usado em conjunto com o comando `order by`.
3. Exemplos de uso: "0s 10 pagamentos mais recentes", "Os 3 produtos mais caros".

### Exercícios

Selecione os nomes de cidade distintas que existem no estado de Minas Gerais em ordem alfabética na tabela `sales.customers`.

```sql
select distinct city
from sales.customers
where state = 'MG'
order by city
```

Selecione o visit_id das 10 compras mais recentes efetuadas na tabela `sales.funnel`.

```sql
select visit_id 
from sales.funnel 
where paid_date is not null
order by paid_date desc
limit 10
```

Selecione todos os dados dos 10 clientes com maior score nascidos após 01/01/2000 na tabela `sales.customers`.

```sql
select *
from sales.customers
where birth_date > '2000-01-01'
order by score desc
limit 10
```

## Operadores aritméticos 

É possível realizar operações matemáticas. **Muito usado para criar colunas calculadas**.

| Operadores aritméticos                  | Descrição       |
| --------------------------------------- | --------------- |
| `+`                                     | Adição          |
| `-`                                     | Subtração       |
| `*`                                     | Multiplicação   |
| `/`                                     | Divisão         |
| `ˆ`                                     | Exponencial     |
| `%`                                     | Módulo          |
| **\|\|** (Não é um operador aritmético) | Soma de strings |

Crie uma coluna contendo a idade do cliente da tabela `sales.customers`. Use aspas duplas para nomear com espaços.

```sql
select
	email,
	birth_date,
	(current_date - birth_date) / 365 as age
	-- (current_date - birth_date) / 365 as "customer age" (Aspas duplas)
from sales.customers
limit 10
```

Liste os 10 clientes mais novos da tabela `sales.customers`.

```sql
select
	email,
	birth_date,
	(current_date - birth_date) / 365 as "customer age"
from sales.customers
order by "customer age"
limit 10
```

Crie a coluna `full_name` contendo o nome completo do cliente.

```sql
select
	first_name,
	last_name,
	first_name || ' ' || last_name as full_name
from sales.customers
limit 10
```

### Resumo

1. Muito usado para criar colunas calculadas.
2.  Pseudônimos são usados para nomear colunas calculadas.
3. Aspas duplas são usadas para pseudônimos que contém espaços.
4. O operador `||` é usado para concatenar strings.

## Operadores de comparação

É possível comparar dois valores retornando TRUE ou FALSE. **Muito usado com a função `where` para filtrar linhas**.

| Operadores de comparação | Descrição          |
| ------------------------ | ------------------ |
| `=`                      | Igual à            |
| `>`                      | Maior que          |
| `<`                      | Menor que          |
| `>=`                     | Maior ou igual que |
| `<=`                     | Menor ou igual que |
| `<>`                     | Diferente de       |

Crie uma coluna que retorne `true` se cliente é um profissional `clt` na tabela `sales.customers`.

```sql
select
	customer_id,
	first_name,
	professional_status,
	(professional_status = 'clt') as clt_customer 
from sales.customers
```

### Resumo

1. Comparar dois valores e retorno será `true` ou `false`.
2. Muito usado com a função `where` para filtrar linhas. 
3. Criar colunas flag que retornam `true` ou `false`.

## Operadores lógicos

Usados para unir expressões simples em uma composta.

| Operadores lógicos | Descrição                                                     |
| ------------------ | ------------------------------------------------------------- |
| `AND`              | TRUE se todos as condições separadas por AND forem TRUE       |
| `OR`               | TRUE se alguma condição separada por OR for TRUE              |
| `NOT`              | TRUE se a condição mencionada anteriormente não for TRUE      |
| `BETWEEN`          | TRUE se o operando estiver dentro do intervalo comparativo    |
| `IN`               | TRUE se o operando for igual a algum item de uma lista        |
| `LIKE`             | TRUE se o operando corresponder a um padrão determinado       |
| `ILIKE`            | Igual ao LIKE mas desconsidera letras maiúsculas e minúsculas |
| `IS NULL`          | TRUE se a condição for igual a NULL                           |

### BETWEEN

Selecione veículos que custam entre 100 mil e 200 mil na tabela `sales.products`.

```sql
select *
from sales.products
where price >= 100000 and price <= 200000
order by price
```

```sql
select *
from sales.products
where price between 100000 and 200000
order by price
```

Selecione veículos que custam abaixo de 100 mil e acima de 200 mil na tabela `sales.products`.

```sql
select *
from sales.products
where price < 100000 or price > 200000
order by price desc
```

Selecione veículos que custam abaixo de 100 mil e acima de 200 mil na tabela `sales.products`.

```sql
select *
from sales.products
where price not between 100000 and 200000
order by price
```

### IN

Selecione produtos das marcas `Honda`, `Toyota` e `Renault` na tabela `sales.products`.

```sql
select *
from sales.products
where brand = 'HONDA' or brand = 'TOYOTA' or brand = 'RENAULT'
```

```sql
select *
from sales.products
where brand in ('HONDA', 'TOYOTA', 'RENAULT')
```

Selecione produtos que não são das marcas `Honda`, `Toyota` e `Renault` na tabela `sales.products`.

```sql
select *
from sales.products
where brand not in ('HONDA', 'TOYOTA', 'RENAULT')
```

### LIKE (Matchs Imperfeitos)

Selecione os clientes distintos com o nome `ANA` na tabela `sales.customers`.

```sql
select distinct first_name
from sales.customers
where first_name = 'ANA'
```

Selecione todos os nomes que iniciam com os caracteres `ANA`.

```sql
select distinct first_name
from sales.customers
where first_name like 'ANA%' 
```

Selecione todos os nomes que terminam com os caracteres `ANA`.

```sql
select distinct first_name
from sales.customers
where first_name like '%ANA' 
```

### IS NULL

Selecione apenas as linhas que contém nulo no campo `population` na tabela `temp_tables.regions`.

```sql
select *
from temp_tables.regions
where population is null
```

### Resumo

#### `AND`

Verifica se duas comparações são simultaneamente verdadeiras.

#### `OR`

Verifica se uma ou outra comparação é verdadeira.

#### `BETWEEN`

Verifica quais valores estão dentro do range definido.

#### `IN`

Funciona como múltiplos ORs

#### `LIKE

Compara textos e é frequentemente usado com o operador `%`.

#### `ILIKE`

Ignora se o campo tem letras maiúsculas ou minúsculas na comparação.

#### `IS NULL`

Verifica se o campo é nulo.

### Exercícios

Calcular quantos salários mínimos cada cliente da tabela `sales.customers` recebe. Selecione as seguintes colunas: `email`, `income` e a coluna calculada `salários mínimos`. Considere o salário mínimo igual à R$ 1.200,00.

```sql
select
	email,
	income,
	round((income) / 1200, 0) as "salários mínimos"
from sales.customers
order by income desc
```

Criar na consulta anterior uma coluna informando `true` se o cliente ganha acima de 4 salários mínimos e `false` se ganha 4 salários ou menos na tabela `sales.customers`. Nomear a nova coluna de "acima de 4 salários".

```sql
select
	email,
	income,
		round((income) / 1200, 0) as "salários mínimos",
	round((income) / 1200, 0) > 4 as "acima de 4 salários"
from sales.customers	
order by income
```

Filtrar na consulta anterior apenas os clientes que ganham entre 4 e 5 salários mínimos na tabela `sales.customers`. Use o comando `BETWEEN`.

```sql
select
	email,
	income,
	round((income) / 1200, 1) as "salários mínimos",
	round((income) / 1200, 1) > 4 as "acima de 4 salários"
from sales.customers
where round((income) / 1200, 1) between 4 and 5
order by income
```

Selecine o `email`, `cidade` e `estado` dos clientes que moram no estado de Minas Gerais e Mato Grosso na tabela `sales.customers`. 

```sql
select email, city, state
from sales.customers
where state in ('MG', 'MT')
order by state desc
```

Selecine o email, cidade e estado dos clientes que não moram no estado de São Paulo na tabela `sales.customers`.

```sql
select email, city, state
from sales.customers
where state not in ('SP')
```

Selecine os nomes das cidade que começam com a letra Z na tabela `temp_table.regions`.

```sql
select city
from sales.customers
where city like 'Z%'
```

---
## Funções de agregação

Usadas para executar operações aritméticas nos registros de uma coluna.

| Funções   | Descrição                    |
| --------- | ---------------------------- |
| `COUNT()` | Contar o número de linhas    |
| `SUM()`   | Calcular a soma dos valores  |
| `MIN`     | Retornar o menor valor       |
| `MAX`     | Retornar o maior valor       |
| `AVG`     | Calcular a média dos valores |

### COUNT

Conte todas as linhas da tabela `sales.funnel`.

```sql
select count(*)
from sales.funnel
```

> É importante, antes de qualquer consulta, a contagem de todas as linhas da tabela para evitar garlgalo durante a consulta, se houver muitas linhas na base.

Conte todos os pagamentos registrados na tabela `sales.funnel`.

```sql
select count(paid_date)
from sales.funnel
```

> `count(paid_date)` não conta os pagamentos que estão como `null`.

Conte todos os produtos distintos visitados no mês de Janeiro de 2021.

```sql
select count(distinct product_id)
from sales.funnel
where visit_page_date between '2021-01-01' and '2021-01-31'
```

> O retorno desta consulta é a visita de 139 produtos distintos. Será que não vale o investimento desses produtos? É uma análise crítica sobre os produtos mais visitados.

### SUM

Consulta para somar todos os preços dos produtos na tabela `sales.products`.

```sql
select sum(price)
from sales.products
```

### MIN, MAX and AVG

Calcule o preço mínimo, máximo e médio dos produtos da tabela `sales.products`.

```sql
select min(price), max(price), avg(price)
from sales.products
```

Consulte o veículo mais caro da tabela `sales.products`.

```sql
select max(price)
from sales.products
```

```sql
select *
from sales.products
where price = (select max(price) from sales.products)
```

> A consulta acima possui subqueries para retornar as informações do produto mais caro 👆🏼.

### Resumo

1.  Funções agregadas não computam células vazias (NULL).
2. É possível usar o asterisco (`*`) com a função `COUNT()` para contar todos os registros.
3. `COUNT(DISTINCT)` irá contar apenas os valores exclusivos.

---
## GROUP BY

Serve para agrupar registros semelhantes de uma coluna. Normalmente usado com as Funções de Agregação.

Calcule o nº de clientes por estado na tabela `sales.custumers`.

```sql
select state, count(*)
from sales.customers
group by state
```

```sql
select state as estado, count(*) as contagem
from sales.customers
group by state
order by contagem desc
```

Calcule o nº de clientes por estado e status profissional na tabela `sales.custumers`.

```sql
select state, professional_status, count(*) as contagem
from sales.customers
group by state, professional_status
order by contagem desc
```

```sql
select state, professional_status, count(*) as contagem
from sales.customers
group by 1, 2
order by contagem desc
```

> O exemplo acima `group by 1, 2` não é recomendado por motivos semânticos.

Selecione os estados distintos na tabela `sales.customers`.

```sql
select distinct state
from sales.customers
```

```sql
select state
from sales.customers
group by state
```

>  O `GROUP BY` pode, assim como o `DISTINCT` eliminar linhas duplicadas.

---
## HAVING

É usado para filtrar linhas da seleção por uma coluna agrupada.

Conte o nº de clientes por estado filtrando apenas estados acima de 100 clientes.

```sql
select
	state,
	count(*)
from sales.customers
group by state
having count(*) > 100
```

Filtrar o nº de clientes dos estados acima de 100 clientes e que não seja de Minas Gerais.

```sql
select
	state,
	count(*)
from sales.customers
where state <> 'MG'
group by state
having count(*) > 100
```

```sql
select
	state,
	count(*)
from sales.customers
group by state
having count(*) > 100
	and state <> 'MG'
```

### Resumo

1. `HAVING` e `WHERE` são usados da mesma maneira, porém o `HAVING` filtra os resultados das Funções Agregadas, diferente do `WHERE` que possui limitações.
2. `HAVING` pode filtrar colunas não agregadas.

### Exercícios

Conte quantos clientes da tabela `sales.customers` tem menos de 30 anos.

```sql
select
	count(*)
from sales.customers
where ((current_date - birth_date) / 365) < 30
```

```sql
select
	(current_date - birth_date) / 365 as "customer age",
	count(*) as contagem
from sales.customers
group by "customer age"
having ((current_date - birth_date) / 365) < 30
order by contagem
```

Informe a idade do cliente mais velho e mais novo da tabela `sales.customers`

```sql
select
	max((current_date - birth_date) / 365) as older,
	min((current_date - birth_date) / 365) as younger
from sales.customers
```

Selecione todas as informações do cliente mais rico da tabela `sales.customers`. Possívelmente a resposta contém mais de um cliente.

```sql
select *
from sales.customers
where income = (select max(income) from sales.customers)
```

Conte quantos veículos de cada marca tem resgistrado na tabela `sales.products`. Ordene o resultado pelo nome da marca.

```sql
select
	brand,
	count(*) as contagem
from sales.products
group by brand
order by contagem desc
```

Conte quantos veículos existem registrados na tabela `sales.products`. Ordene pelo nome da marca e ano do veículo.

```sql
select
	brand,
	model_year,
	count(*)
from sales.products
group by brand, model_year
order by brand, model_year
```

Conte quantos veículos de cada marca tem registrado na tabela `sales.products` e mostre apenas as marcas que contém mais de 10 veículos registrados.

```sql
select
	brand,
	count(*) as contagem
from sales.products
group by brand
having count(*) > 10
```

---
## JOIN

É usado para relacionar informações entre tabelas e existem 4 tipos de Joins. Os mais usados são `LEFT JOIN` e `INNER JOIN`.

![Joins](Joins.png)

### LEFT JOIN

Use o LEFT JOIN para o relacionamento entre as tabelas `temp_tables.tabela_1` e `temp_tables.table_2`.

```sql
select t1.cpf, t1.name, t2.state
from temp_tables.tabela_1 as t1 
left join temp_tables.tabela_2 as t2
	on t1.cpf = t2.cpf
```

 O consulta irá retornar todos os itens possíveis da tabela `temp_tables.tabela_1 (left)` e a tabela `temp_tables.tabela_2 (right)` apenas os itens que derem match com a tabela `temp_tables.tabela_1 (left)`.  Os itens da `temp_tables.tabela_2 (right)` que não derem match, irão retornar nulos.

> Um ponto positivo do `LEFT JOIN` é que ele retorna a quantidade de dados nulos. Importante para entender a divergência entre as tabelas. Ao contrário do `INNER JOIN` que retorna apenas os dados que derem match.

### INNER JOIN

Use o INNER JOIN para o relacionamento entre as tabelas `temp_tables.tabela_1` e `temp_tables.table_2`.

```sql
select t1.cpf, t1.name, t2.state
from temp_tables.tabela_1 as t1 
inner join temp_tables.tabela_2 as t2
	on t1.cpf = t2.cpf
```

 O consulta irá retornar apenas os itens que derem match na `temp_tables.tabela_1 (left)` e `temp_tables.tabela_2 (right)`. Sendo assim, o `INNER JOIN` não retorna campos nulos.

### RIGHT JOIN

Use o RIGHT JOIN para o relacionamento entre as tabelas `temp_tables.tabela_1` e `temp_tables.table_2`.

```sql
select t2.cpf, t2.state, t1.name
from temp_tables.tabela_1 as t1 
right join temp_tables.tabela_2 as t2
	on t1.cpf = t2.cpf
```

O consulta irá retornar todos os itens possíveis da tabela `temp_tables.tabela_2 (right)` e a tabela `temp_tables.tabela_1 (left)` apenas os itens que derem match com a tabela `temp_tables.tabela_2 (right)`.  Os itens da `temp_tables.tabela_1 (left)` que não derem match, irão retornar nulos.

### FULL JOIN

Use o FULL JOIN para o relacionamento entre as tabelas `temp_tables.tabela_1` e `temp_tables.table_2`.

```sql
select t2.cpf, t2.state, t1.name
from temp_tables.tabela_1 as t1 
full join temp_tables.tabela_2 as t2
	on t1.cpf = t2.cpf
```

O consulta irá retornar todos os itens que derem match e também os itens nulos entre a `temp_tables.tabela_1 (left)` e `temp_tables.tabela_2 (right)`.

### Exercícios

Identifique qual é o status profissional dos clientes que compraram automóveis no site.

```sql
select customers.first_name, customers.state, customers.professional_status, funnel.paid_date
from sales.customers as customers
inner join sales.funnel as funnel
	on customers.customer_id = funnel.customer_id
where funnel.paid_date is not null
```

Identifique qual é o status profissional mais frequente dos clientes que compraram automóveis no site.

```sql
select customers.professional_status, count(funnel.paid_date) as payments
from sales.customers as customers
inner join sales.funnel as funnel
	on customers.customer_id = funnel.customer_id
where funnel.paid_date is not null
group by customers.professional_status
order by payments desc
```

Identifique qual é o gênero mais frequente dos clientes que compraram automóveis no site. Você deve usar a tabela `temp_tables.ibge_genders`.

```sql
select 
	ibge.gender,
	count(fun.paid_date) as payments
from sales.customers as cus
left join sales.funnel as fun
	on cus.customer_id = fun.customer_id
left join temp_tables.ibge_genders as ibge
	on lower(cus.first_name) = ibge.first_name
group by ibge.gender
order by payments desc
```

Identifique de quais regiões são os clientes que mais visitam o site. Você deve usar a tabela `temp_tables.regions`.

```sql
select
	reg.region,
	count(fun.visit_page_date) as visitas
from sales.funnel as fun
left join sales.customers as cus
	on fun.customer_id = cus.customer_id
left join temp_tables.regions as reg
	on lower(cus.city) = lower(reg.city)
	and lower(cus.state) = lower(reg.state)
where reg.region is not null
group by reg.region
order by visitas desc
```

Identifique quais as marcas de veículos mais visitadas na tabela `sales.funnel`.

```sql
select
	prod.brand as brand,
	count(*) as visits
from sales.funnel as fun
left join sales.products as prod
	on fun.product_id = prod.product_id
group by brand
order by visits desc
```

Identifique quais as lojas de veículo mais visitadas na tabela `sales.funnel`

```sql
select
	st.store_name as store,
	count(*) as visits
from sales.funnel as fun
left join sales.stores as st
	on fun.store_id = st.store_id
group by store
order by visits desc
```

Identifique quantos clientes moram em cada tamanho de cidade. O porte da cidade consta na coluna `size` da tabela `temp_tables.regions`.

```sql
select
	reg.size as "region size",
	count(*) as customers
from sales.customers as cus
left join temp_tables.regions as reg
	on lower(cus.city) = lower(reg.city)
	and lower(cus.state) = lower(reg.state)
group by "region size"
order by customers desc
```

---
## UNION

É usado para colar uma tabela sobre a outra, desde que as duas tabelas tenham a mesma quantidade de colunas e essas colunas possuam o mesmo tipo de unidade.

### Exercícios

Unir a tabela `sales.products` e a tabela `temp_tables.products_2`.

```sql
select * from sales.products
union all
select * from temp_tables.products_2
```

---
## SUBQUERY

É usado para consultar dados de outras consultas.

### Tipos

-  Subquery no `WHERE`
-  Subquery com `WITH`
-  Subquery no `FROM`
-  Subquey no `SELECT`

#### Subquery no `WHERE`

Identifique qual é o veículo mais barato da tabela `sales.products`.

```sql
select *
from sales.products
where price = (select min(price) from sales.products)
```

#### Subquery com `WITH`

Calcule a idade média dos clientes por status profissional na tabela `sales.customers`.

```sql
with tabela as (
select 
	professional_status,
	(current_date - birth_date) / 365 as idade
from sales.customers )

select 
	professional_status,
	avg(idade) as idade_media
from tabela
group by professional_status
```

> A Subquery `WITH` é muito usada para trabalhar com análise de dados. Normalmente, quando temos acesso a uma base de dados, a primeira etapa é realizar a transformação desses dados e em seguida, aplicar as consultas para análise.

#### Subquery no `FROM`

Calcule a média das idades dos clientes por status profissional.

```sql
select 
	professional_status,
	avg(idade) as idade_media
from (
	select 
		professional_status,
		(current_date - birth_date) / 365 as idade
	from sales.customers
) as tabela
group by professional_status
```

#### Subquery no `SELECT`

Crie uma coluna na tabela `sales.funnel` que informe o nº de visitas acumuladas que a loja visitada recebeu até o momento.

```sql
select
	fun.visit_id,
	fun.visit_page_date,
	sto.store_name,
	(
		select count(*)
		from sales.funnel as funnel
		where funnel.visit_page_date <= funnel.visit_page_date
			and funnel.store_id = fun.store_id
		
	) as visitas_acumuladas

from sales.funnel as fun
left join sales.stores as sto
	on fun.store_id = sto.store_id
order by sto.store_name, fun.visit_page_date
```

### Exercícios

Análise de recorrência dos leads. Calcule o volume de visitas por dia ao site separado por 1º visita (`false`) e mais de uma visita (`true`).

```sql
with primeira_visita as (
	select customer_id, min(visit_page_date) as visita_1
	from sales.funnel
	group by customer_id	
)

select
	fun.visit_page_date,
	(fun.visit_page_date <> primeira_visita.visita_1) as lead_recorrente,
	count(*)

from sales.funnel as fun
left join primeira_visita
	on fun.customer_id = primeira_visita.customer_id
group by fun.visit_page_date, lead_recorrente
order by fun.visit_page_date desc, lead_recorrente
```

Análise do preço vs preço médio. Comparar para cada visita ao site, o preço do veículo visitado pelo cliente e o preço médio para o veículo daquela marca. Verificar se o valor está acima ou abaixo da média. Levar em consideração o desconto dado no veículo.

```sql
with preco_medio as (
	select brand, avg(price) as preco_medio_da_marca
	from sales.products
	group by brand
)

select
	fun.visit_id,
	fun.visit_page_date,
	pro.brand,
	(pro.price * (1 + fun.discount)) as preco_final,
	preco_medio.preco_medio_da_marca,
	((pro.price * (1 + fun.discount)) - preco_medio.preco_medio_da_marca) as preco_vs_media
from sales.funnel as fun
left join sales.products as pro
	on fun.product_id = pro.product_id
left join preco_medio
	on pro.brand = preco_medio.brand
```

Crie uma coluna calculada com o nº de visitas realizadas por cada cliente da tabela `sales.customers`.

```sql
with number_of_visits as (
	select customer_id, count(*) as visits
	from sales.funnel
	group by customer_id
)

select
	c.*,
	n.visits
from sales.customers as c
left join number_of_visits as n
	on c.customer_id = n.customer_id
order by n.visits desc
```
### Resumo

1.  Subqueries no `WHERE` e no `SELECT` devem retornar apenas um único valor.
2.  Não é recomendado usar subqueries diretamente dentro do `FROM`, pois isso dificulta a legibilidade da consulta.

---

## Conversão de Unidades

Conversão de texto em data usando o operador `::`.

```sql
select '2026-10-01'::date - '2026-02-01'::date
```

Conversão de texto em número usando o operador `::`.

```sql
select '100'::numeric - '10'::numeric
```

Conversão de número em texto usando o operador `::`

```sql
select replace(112122::text, '1', 'A')
```

Conversão de texto em data usando a função `CAST`.

```sql
select cast('2026-10-01' as date) - cast('2026-02-01' as date)
```
### Resumo

1.  O operador `::` e o `CAST` são funções utilizadas para converter o dado para algum tipo específico.
2. O operador `::` é mais clean, porém, em algumas ocasiões não funcionado como deveria. Sendo assim, o `CAST` pode ser uma alternativa.

---
## Tratamento Geral
### CASE WHEN

Agrupamento de dados. Calcule o nº de clientes que ganham abaixo de R$ 5.000, entre R$ 5.000 e R$ 10.000, entre R$ 10.000 e R$ 15.000 e acima de R$ 15.000.

```sql
with faixa_de_renda as (
	select
		income,
		case
			when income < 5000 then '0-5000'
			when income >= 5000 and income < 10000 then '5000-10000'
			when income >= 10000 and income < 15000 then '10000-15000'
			else '15000+'
			end as faixa_renda
	from sales.customers
)

select faixa_renda, count(*) as customers
from faixa_de_renda
group by faixa_renda
order by customers desc
```

### Resumo

1. `CASE WHEN` é o comando usado para criar condições específicas. É muito utilizado no agrupamento de dados.

### COALESCE

Tratamento de dados. Crie uma coluna chamada `populacao_ajustada` na tabela `temp_tables.regions` e preencha com os dados da coluna `population`. Porém, se o campo estiver nulo, preencha com a população média (geral) das cidades do Brasil.

```sql
select
	*,
	case
		when population is not null then population
		else (select avg(population) from temp_tables.regions)
		end as populacao_ajustada
from temp_tables.regions
```

```sql
select
	*,
	coalesce(population, (select avg(population) from temp_tables.regions)) as populacao_ajustada
from temp_tables.regions
```

### Resumo

1. `COALESCE` é o comando usado para preencher campos nulos com o primeiro valor não nulo de uma sequência de valores.

---
## Tratamento de Texto

```sql
select upper('São Paulo') = 'SÃO PAULO'

select lower('São Paulo') = 'são paulo'

select trim('SÃO PAULO    ') = 'SÃO PAULO'

select replace('SAO PAULO', 'SAO', 'SÃO') = 'SÃO PAULO'
```

### Resumo

1. `LOWER()` é usado para transformar todo texto em letras minúsculas.
2. `UPPER()` é usado para transformar todo texto em letras maiúsculas.
3. `TRIM()` é usado para remover os espaços das extremidades de um texto.
4. `REPLACE()` é usado para substituir uma `string` por outra `string`.

---
## Tratamentos de Datas

### INTERVAL

Soma de datas. Calcule a data de hoje mais 10 unidades (dias, semanas, meses e horas).

```sql
select current_date + 10 -- +10 dias

select (current_date + interval '10 weeks')::date -- +10 semanas

select (current_date + interval '10 months')::date -- +10 meses

select current_date + interval '10 hours' -- +10 horas
```

### DATE TRUNC

Trucagem de datas. Calcule quantas visitas ocorreram por mês no site da empresa.

```sql
select 
	date_trunc('month', visit_page_date)::date as date_trunc,
	count(*) as count
from sales.funnel
group by date_trunc
order by count desc
```

### EXTRACT

Extração de unidades. Calcule qual é o dia da semana que mais recebe visita ao site.

```sql
select 
	extract('dow' from visit_page_date) as dia_da_semana,
	count(*) as count
from sales.funnel
group by dia_da_semana
order by count desc
```

### Diferença entre datas

Calcule a diferença entre a data de hoje e `2018-04-06`, em dias, semanas, meses e anos.

```sql
select (current_date - '2018-04-06') -- Dias

select (current_date - '2018-04-06')/7 -- Semanas

select (current_date - '2018-04-06')/30 -- Meses

select (current_date - '2018-04-06')/365 -- Anos
```

### Resumo

1. O comando `INTERVAL` é usado para somar datas com unidades diferentes.
2. O comando `DATE TRUNC` é usado para truncar uma data no início do período.
3. O comando `EXTRACT` é usado para extrair unidades de uma data.
4. O cálculo da diferença entre datas com o operador `-`, retorna valores em dias. Para calcular a diferença entre datas em outra unidade, é necessário aplicar uma transformação de unidades (ou criar uma função específica para isso).

---
## Funções

Servem para criar comandos personalizados de scripts usados de forma recorrente.

Crie uma função chamada `DATEDIFF` para calcular a diferença entre duas datas em dias, semanas, meses e anos.

```sql
create function datediff(unidade varchar, data_inicial date, data_final date)
returns integer
language sql

as

$$
	select 
		case
			when unidade in ('d', 'day' ,'days') then (data_final - data_inicial)
			when unidade in ('w', 'week' ,'weeks') then (data_final - data_inicial) / 7
			when unidade in ('m', 'month' ,'months') then (data_final - data_inicial) / 30
			when unidade in ('y', 'year' ,'years') then (data_final - data_inicial) / 365
			end as diferenca
$$




select datediff('day', '2018-04-06', current_date) -- Dias

select datediff('week', '2018-04-06', current_date) -- Semanas

select datediff('month', '2018-04-06', current_date) -- Meses

select datediff('year', '2018-04-06', current_date) -- Anos
```


Delete a função `DATEDIFF` criada no exercício anterior.

```sql
drop function datediff
```

### Resumo

1. `CREATE FUNCTION` é usado par criar funções.
2. O script da função deve ser escrito entre `$$`.

---
## Criação e Deleção

Criação de tabela a partir de uma consulta. Crie uma tabela chamada customers_age com o `id` e a `idade` dos clientes. O caminho deve ser `temp_tables.customers_age`.

```sql
select
	customer_id,
	((current_date - birth_date) / 365) as idade_cliente
	into temp_tables.customers_age
from sales.customers
```

```sql
select * from temp_tables.customers_age limit 10
```

Criar nova tabela. Crie uma tabela com a tradução dos status profissionais dos clientes. O caminho deve ser `temp_tables.profissoes`.

```sql
create table temp_tables.profissoes (
	professional_status varchar,
	status_profissional varchar
)
```

```sql
insert into temp_tables.profissoes
(professional_status, status_profissional)

values
('freelancer', 'freelancer'),
('retired', 'aposentado(a)'),
('clt', 'clt'),
('self_employed', 'autônomo(a)'),
('other', 'outro'),
('businessman', 'empresário(a)'),
('civil_servant', 'funcionário(a) público(a)'),
('student', 'estudante')
```

```sql
select * from temp_tables.profissoes
```

Deleção de tabelas. Delete a tabela `temp_tables.profissoes`.

```sql
drop table temp_tables.profissoes
```

### Resumo

1.  O comando `INTO` com o `schema.table` antes do `from` é usado para criar tabelas a partir de uma consulta.
2. `CREATE TABLE` é usado para criar novas tabelas. `INSERT INTO` para definir as colunas e os tipos dos valores que serão inseridos. `VALUES` para definir os valores que serão inseridos de forma manual em formato de lista.

---
## Linhas - Inserção, Atualização e Deleção

Insira os status `desempregado(a)` e `estagiário(a)` na `temp_tables.profissoes`.

```sql
create table temp_tables.profissoes (
	professional_status varchar,
	status_profissional varchar
)
```

```sql
insert into temp_tables.profissoes
(professional_status, status_profissional)

values
('freelancer', 'freelancer'),
('retired', 'aposentado(a)'),
('clt', 'clt'),
('self_employed', 'autônomo(a)'),
('other', 'outro'),
('businessman', 'empresário(a)'),
('civil_servant', 'funcionário(a) público(a)'),
('student', 'estudante')
```

```sql
insert into temp_tables.profissoes
(professional_status, status_profissional)

values
('unemployed', 'desempregado(a)'),
('trainee', 'estagiário(a)')
```

```sql
select * from temp_tables.profissoes
```

Atualização de linhas. Corrigir a tradução de `estagiário(a)` de `trainee` para `intern`.

```sql
update temp_tables.profissoes
set professional_status = 'intern'
where status_profissional = 'estagiário(a)'
```

```sql
select * from temp_tables.profissoes
```

Deleção de linhas. Delete as linhas dos status `desempregado(a)` e `estagiário(a)`.

```sql
delete from temp_tables.profissoes
where status_profissional = 'desempregado(a)'
or status_profissional = 'estagiário(a)'
```

```sql
select * from temp_tables.profissoes
```

---
## Colunas - Inserção, Atualização e Deleção

Inserção de colunas. Insira uma coluna na tabela `sales.customers` com a idade do cliente.

```sql
alter table sales.customers
add customer_age int
```

```sql
update sales.customers
set customer_age = (current_date - birth_date) / 365
where true
```

```sql
select * from sales.customers
```

Alteração do tipo da coluna. Altere o tipo da coluna `customer_age` de `int` para `varchar`.

```sql
alter table sales.customers
alter column customer_age type varchar
```

```sql
select * from sales.customers
```

Alteração do nome da coluna. Renomeie o nome da coluna `customer_age` para `age`.

```sql
alter table sales.customers
rename column customer_age to age
```

```sql
select * from sales.customers
```

Deleção da coluna. Delete a coluna `age`.

```sql
alter table sales.customers
drop column age
```

```sql
select * from sales.customers
```

---
