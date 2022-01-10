# SQL
A few SQL Queries / Algumas Querys SQL


SELECT `Venda`.`COD_CONTROLE_VENDA` AS 'Codigo de Controle',<br>
       `Venda`.`DT_VENDA` AS 'Data da Venda',<br>
       `Cliente`.`NOME` AS 'Nome do Cliente',<br>
       `Venda`.`VLR_TOTAL` AS 'Valor do Contrato',<br>
       CONCAT('Parcela ', `ParcelaDocFin`.`NUM_PARCELA`) AS 'Origem do Pagamento',<br>
       `ParcelaDocFin`.`VLR_QUITADO` AS 'Valor do Pagamento',<br>
       DATE_FORMAT(`ParcelaDocFin`.`DT_VENC`, '%d/%m/%Y') AS 'Data de Vencimento',<br>
       DATE_FORMAT(`ParcelaDocFin`.`DT_MOV`, '%d/%m/%Y') AS 'Data do Pagamento',      
       ROUND((`ParcelaDocFin`.`VLR_QUITADO` * 0.39), 2) AS 'Comissao Socio1',<br>
       ROUND((`ParcelaDocFin`.`VLR_QUITADO` * 0.0915), 2) AS 'Comissao Socio2',<br>
       ROUND((`ParcelaDocFin`.`VLR_QUITADO` * 0.0915), 2) AS 'Comissao Socio3',<br>
       ROUND((`ParcelaDocFin`.`VLR_QUITADO` * 0.427), 2) AS 'Comissao Socio4'      
FROM `DocFin_DocVenda`, `ParcelaDocFin`, `Venda`, `Cliente`<br>
WHERE `DocFin_DocVenda`.`COD_DOC_FIN` = `ParcelaDocFin`.`COD_DOC_FIN`<br>
AND `DocFin_DocVenda`.`COD_VENDA` = `Venda`.`COD_VENDA`<br>
AND `Cliente`.`ID` = `Venda`.`COD_CLIENTE`<br>
AND `ParcelaDocFin`.`DT_MOV` BETWEEN '2021-10-01' AND '2021-10-31'<br>
ORDER BY `ParcelaDocFin`.`DT_MOV`;<br>
<br><br>

SELECT `venda`.`COD_CONTROLE_VENDA` AS 'Codigo de Controle',<br>
       `cliente`.`NOME` AS 'Nome do Cliente',<br>
       `venda`.`VLR_TOTAL` AS 'Valor do Contrato',<br>
       `parceladocfin`.`NUM_PARCELA` AS 'Origem do Pagamento',<br>
       `parceladocfin`.`VLR_PARCELA` AS 'Valor da Parcela',<br>
       `parceladocfin`.`DT_VENC` AS 'Data de Vencimento'<br>
FROM `docfin_docvenda`, `parceladocfin`, `venda`, `cliente`<br>
WHERE `docfin_docvenda`.`COD_DOC_FIN` = `parceladocfin`.`COD_DOC_FIN`<br>
AND `docfin_docvenda`.`COD_VENDA` = `venda`.`COD_VENDA`<br>
AND `cliente`.`ID` = `venda`.`COD_CLIENTE`<br>
AND `parceladocfin`.`FORMA_PGTO` = 'Em Aberto'<br>
ORDER BY `cliente`.`NOME`, `parceladocfin`.`NUM_PARCELA`;<br>
<br><br>

SELECT *, (CURDATE() - INTERVAL 90 DAY) AS diff<br>
FROM `parceladocfin`<br>
WHERE `parceladocfin`.`FORMA_PGTO` = 'Em Aberto'<br>
AND `parceladocfin`.`DT_VENC` <= CURDATE() - INTERVAL 90 DAY<br>
GROUP BY `parceladocfin`.`COD_DOC_FIN`<br>
ORDER BY `parceladocfin`.`DT_VENC` DESC;<br>
