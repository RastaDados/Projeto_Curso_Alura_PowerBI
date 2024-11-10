# Empresa de Bebidas - Alura PowerBI

<h3>Link do Projeto</h3>
<a href="https://app.powerbi.com/groups/me/reports/c98c2d6a-081d-451a-9b7d-876a0bb8c2f4/93d01abe0b974a8685d0?experience=power-bi"> Veja o Dashboard aqui </a>
<hr>

<h3>Conhecendo a base de dados</h3>

![Base_Dados](https://github.com/user-attachments/assets/8ab3494f-2477-44a3-9383-e1126a85aa69)

<br>

Como podemos visualizar, a base de dados utilizada é de uma empresa que vende bebidas (Refiregerantes e Sucos).
Onde temos as respectivas tabelas:

* Clientes
* Item de Notas Fiscais
* Notas Fiscais
* Produtos
* Vendedores
* Clientes Auxiliar Parte 2
* Clientes Parte 2
* Notas Parte 2
* Items de Notas Fiscais Parte 2
* Clientes Auxiliar
<br>
Todas as tabelas que contém auxiliar ou parte 2, foram adicionadas apenas para atualizar a base de dados, alimentando-a com mais informações.  
<br>

Há também uma tabela criada para servir como inteligência de tempo, que foi a famosa "dCalendario".
<br>
<hr>

<h2> Medidas DAX </h2>

Foram criadas medidas utilizando a linguagem <b>DAX</b> (Data Analysis Expressions)

![Medidas](https://github.com/user-attachments/assets/94950cdb-c17a-4adc-a5cb-db27d9230bc2)

* % de Imposto

  
  DIVIDE([Imposto em R$], [Total Imposto])
  Esta medida tem como função exibir a porcentagem de imposto total
  Fiz a divisão entre as medidas criadas <b>Imposto em R$</b> e <b>Total Imposto</b> para pegar a porcentagem (%) de impostos.

* % Imposto Acumulado

  
  DIVIDE([Imposto Acumulado], CALCULATE([Imposto em R$], ALL(Produtos[Nome_Produto])))
  Essa medida tem como função exibir a porcentagem do imposto acumulado por produtos.
  Fiz a divisão da medida criada <b>Imposto Acumulado</b>, utilizando a CALCULATE para buscar os valores de <b>Impostos em R$</b> e a função ALL para puxar todas os linhas sem filtro da coluna <b>Nome Produto</b> da tabela <b>Produtos</b>

* % Produtos

  CALCULATE([Posição No Ranking] / [Total Produtos])]
  Essa medida tem como função exibir a porcentagem de produtos por ranking de vendas
  Utilizo a função CALCULATE para buscar os dados da medida <br>Posição no Ranking</b> e dividir pelo total de produtos que está na medida <b> Total Produtos </b>

* Faturamento LY
  
  CALCULATE(SUMX('Items de Notas Fiscais', 'Items de Notas Fiscais'[Faturamento]), SAMEPERIODLASTYEAR(dCalendario[Data_Venda]))
  Essa medida tem como função exibir o faturamento em comparação com o LY (Last Year / Ano Anterior)
  Utilizo a função CALCULATE para buscar os dados da medida <b>Items de Notas Fiscais </b> e da coluna <b> Faturamento </b> que está na tabela <b> Items de Notas Fiscais </b> e dentro da CALCULATE eu coloco a função SUMX para fazer a soma linha a linha da medida e da coluna acima, e por fim a função SAMEPERIODLASTYEAR para buscar os dados do ano anterior, passando como parâmetro a coluna de data  <b>Data_Venda</b> da tabela <b>dCalendario</b>.

* Imposto Acumulado
 
  CALCULATE([Imposto em R$], TOPN([Posição No Ranking], ALL(Produtos[Nome_Produto]), [Imposto em R$]))
  Essa medida tem como função exibir o imposto acumulado de todos os produtos por ranking do menor para o maior em quantidade de imposto.
  Utilizo a função CALCULATE para trazer os dados da medida <b>Imposto em R$</b> depois uso a função TOPN para trazer o rank da medida <b>Posição no Ranking</b> e por fim uso a função ALL para trazer todas as linhas ignorando o filtro da coluna <b>Nome Produto</b> e da medida <b:Imposto em R$</b>

* Imposto em R$

  SUMX('Items de Notas Fiscais', 'Items de Notas Fiscais'[Faturamento] - 'Items de Notas Fiscais'[Faturamento_Sem_Imposto])
  Essa medida tem como função exibir a quantidade de imposto em reais.
  Utilizo a função SUMX para realizar a soma da tabela <b>Items de Notas Fiscais</b> e a coluna <b>Faturamento</b> da tabela dita anteriormente, depois faço a  subtração com a coluna <b>Faturamento Sem Imposto</b> da mesma tabela.

* Posição no Ranking

  RANKX(ALL(Produtos[Nome_Produto]), [Imposto em R$])
  Essa medida tem como função exibir o Ranking dos produtos que contém maior imposto em reais
  Utilizo a função RANKX para trazer o ranking dos dados da coluna <b>Nome Produto</b> da tabela <b>Produtos</b> e da medida <b>Imposto em R$</b>

* Qtd Litros Acumulado

  SUM('Items de Notas Fiscais'[Quantidade_Litros])
  Essa medida tem como função exibir a quantidade de litros acumulado dos produtos já vendidos.
  Utilizo a função SUM para somar os dados da coluna <b>Quantidade Litros</b> da tabela <b>Items de Notas Fiscais</b>

* Total Impostos

  SUMX(ALL('Items de Notas Fiscais'), 'Items de Notas Fiscais'[Faturamento] - 'Items de Notas Fiscais'[Faturamento_Sem_Imposto])
  Essa medida tem como função exibir a quantidade todal de impostos
  Utilizo a função SUMX para fazer a soma, trazendo a função ALL para buscar todas as linhas sem filtro da tabela <b>Items de Notas Fiscais</b> e a coluna <b>Faturamento</b> da tabela mencionada, depois subtraio os valores com a coluna <b>Faturamento Sem Imposto</b> da mesma tabela.

* Total Produtos

   MAXX(ALL(Produtos[Nome_Produto]), RANKX(ALL(Produtos[Nome_Produto]),  [Imposto em R$]))
  Essa medida tem como função exibir o número total de produtos
  Utilizo a função MAXX para buscar o valor máximo, depois utilizo a função ALL para buscar todas as linhas sem filtro da coluna <b>Nome Produto</b> da tabela <b>Produtos</b>, depois utilizo a finção RANKX para trazer o ranking de todos os produtos com a função ALL da coluna <b>Nome Produto</b> da coluna <b>Produtos</b> e por fim trago a medida <b>Imposto em R$</b>

<hr>
 
<h2> Página de Faturamento e Imposto </h2>
Essa página foi criada com o intuito de retirar <b>insights</b> e <b>análises</b> dos dados de impostos e faturamento, podendo ser filtrado pelos anos, produtos e categorias (marca). Todos os dados financeiros principais estão nessa página.

<br>
<br>

![01](https://github.com/user-attachments/assets/4b4e308e-5260-407d-8e77-01c6a5476e85)

<hr>

<br>
<br>


<h2>Página de Mapa de Faturamento Por Família do Produto Selecionado</h2>
Essa página foi criada com o intuito de retirar <b>insights</b> e <b>análises</b> dos dados de faturamento, podendo filtrar por ano, tamanho do produto (quantidade de litro da emabalagem), sabor do produto, vendedores específicos da venda e a família (marca) do produto.

<br>
<br>

![02](https://github.com/user-attachments/assets/12b73f67-d8b2-4c65-97ec-1b25c2d3074a)

<hr>

<br>
<br>


<h2>Página de Faturamento Família e Produto Selecionado</h2>
Essa página foi criada com o intuito de retirar <b>insights</b> e <b>análises</b> dos dados de faturamento de produtos e marca do produto, podendo filtrar por ano, vendedor que realizou a venda, sabor do produto, e tamanho da embalagem.

<br>
<br>

![03](https://github.com/user-attachments/assets/7d0243e1-de51-4f72-a771-784e065f86a7)

<hr>

<br>
<br>


<h2>Página de Top 5 Clientes por Top 10 Produtos</h2>
Essa página foi criada com o intuito de retirar <b>insights</b> e <b>análises</b> dos dados de faturamento das marcas e dos produtos, ela traz um rank dos Top 5 Clientes que mais compraram (em termos financeiros e não de quantidade) e dos Top 10 marcas e produtos que mais lucraram. Esses dados podem ser filtrados por ano, Vendedores que realizaram a venda, tamanho da embalagem do produto e sabor do produto.

<br>
<br>

![04](https://github.com/user-attachments/assets/8ab54435-8f2c-4a8a-9c27-8061238ae68e)

<hr>





