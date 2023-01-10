select * from precos;

delete cod_prod
where cod_prod is not null;

/*Pesquise os itens que foram vendidos sem desconto. As colunas presentes no resultado da consulta são: ID_NF, ID_ITEM, COD_PROD E VALOR_UNIT */

select id_nf,id_item,cod_prod,valor_unit,desconto from precos
where desconto is not null;


/* Pesquise os itens que foram vendidos com desconto. As colunas presentes no resultado da consulta são:
 ID_NF, ID_ITEM, COD_PROD, VALOR_UNIT E O VALOR VENDIDO. OBS: O valor vendido é igual ao 
 VALOR_UNIT - (VALOR_UNIT*(DESCONTO/100)).achar valor vendido onde vendido= valor_unit-(valor_unit*(desconto/100))*/

select id_nf, id_item, cod_prod, valor_unit, valor_unit-(valor_unit*(desconto/100)) as valor_vendido
from precos
where desconto>=1;


/*Altere o valor do desconto (para zero) de todos os registros onde este campo é nulo.*/

UPDATE precos
SET desconto=0
WHERE desconto IS NULL;

select * from precos;

/*
Pesquise os itens que foram vendidos. As colunas presentes no resultado da consulta são:
 ID_NF, ID_ITEM, COD_PROD, VALOR_UNIT, VALOR_TOTAL, DESCONTO, VALOR_VENDIDO. 
 OBS: O VALOR_TOTAL é obtido pela fórmula: QUANTIDADE * VALOR_UNIT. O VALOR_VENDIDO é igual a VALOR_UNIT - (VALOR_UNIT*(DESCONTO/100)).
 */
 
 select id_nf,id_item,cod_prod,valor_unit,quant*valor_unit As valor_total,desconto,valor_unit-(valor_unit*(desconto/100)) AS valor_vendido
  FROM precos;
  
/* Pesquise o valor total das NF e ordene o resultado do maior valor para o menor. 
As colunas presentes no resultado da consulta são: ID_NF, VALOR_TOTAL. OBS:	*/
 
SELECT id_nf,valor_unit * quant AS valor_total
FROM precos
GROUP BY valor_total DESC;
  
 /*Pesquise o valor vendido das NF e ordene o resultado do maior valor para o menor.
 As colunas presentes no resultado da consulta são: ID_NF, VALOR_VENDIDO. OBS:
 *O VALOR_TOTAL é obtido pela fórmula: Σ QUANTIDADE * VALOR_UNIT. O VALOR_VENDIDO é igual a Σ VALOR_UNIT - (VALOR_UNIT*(DESCONTO/100)). 
 Agrupe o resultado da consulta por ID_NF.*/
 
 SELECT id_nf, valor_unit-(valor_unit*(desconto/100)) AS valor_vendido, quant*( valor_unit-(valor_unit*(desconto/100))) AS valor_total
 FROM precos
 ORDER BY id_nf;
 

 /* Consulte o produto que mais vendeu no geral. As colunas presentes no resultado da consulta são: COD_PROD, QUANTIDADE. Agrupe o resultado da consulta por COD_PROD.
*/
SELECT cod_prod, sum(quant)  AS quantidade
FROM precos
GROUP BY cod_prod;
 
 
/* Consulte as NF que foram vendidas mais de 10 unidades de pelo menos um produto.
 As colunas presentes no resultado da consulta são: ID_NF, COD_PROD, QUANTIDADE. Agrupe o resultado da consulta por ID_NF, COD_PROD 
*/ 
 select id_nf, cod_prod, quant from precos
 where quant>=10
 group by id_nf, cod_prod;
 
/*Pesquise o valor total das NF, onde esse valor seja maior que 500, e ordene o resultado do maior valor para o menor.
 As colunas presentes no resultado da consulta são: ID_NF, VALOR_TOT. OBS: O VALOR_TOTAL é obtido pela fórmula:
  Σ QUANTIDADE * VALOR_UNIT. Agrupe o resultado da consulta por ID_NF.*/
  
  SELECT *, quant*valor_unit AS soma
  FROM precos
  WHERE quant*valor_unit>=500;
  
  /*Qual o valor médio dos descontos dados por produto.
  As colunas presentes no resultado da consulta são: COD_PROD, MEDIA. Agrupe o resultado da consulta por COD_PROD.
  	*/
    
  select cod_prod, avg(desconto) from precos
  group by cod_prod;
  
 /* Qual o menor, maior e o valor médio dos descontos dados por produto.
 As colunas presentes no resultado da consulta são: COD_PROD, MENOR, MAIOR, MEDIA. Agrupe o resultado da consulta por COD_PROD.
*/
 
 select cod_prod,count(quant), MIN(desconto) as desconto_minimo,
 MAX(desconto) as desconto_maximo,
 AVG(desconto) as media_desconto
 from precos
 group by cod_prod;
 
/*Quais as NF que possuem mais de 3 itens vendidos. As colunas presentes no resultado da consulta são: 
ID_NF, QTD_ITENS. OBS:: NÃO ESTÁ RELACIONADO A QUANTIDADE VENDIDA DO ITEM E SIM A QUANTIDADE DE ITENS POR NOTA FISCAL. Agrupe o resultado da consulta por ID_NF.*/

select id_nf, quant from precos
group by id_nf;
 
  
  
 





