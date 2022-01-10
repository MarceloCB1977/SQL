# SQL
A few SQL Queries / Algumas Querys SQL


SELECT `Venda`.`COD_CONTROLE_VENDA` AS 'Codigo de Controle',
       `Venda`.`DT_VENDA` AS 'Data da Venda',
       `Cliente`.`NOME` AS 'Nome do Cliente',
       `Venda`.`VLR_TOTAL` AS 'Valor do Contrato',
       CONCAT('Parcela ', `ParcelaDocFin`.`NUM_PARCELA`) AS 'Origem do Pagamento',
       `ParcelaDocFin`.`VLR_QUITADO` AS 'Valor do Pagamento',
       DATE_FORMAT(`ParcelaDocFin`.`DT_VENC`, '%d/%m/%Y') AS 'Data de Vencimento',
       DATE_FORMAT(`ParcelaDocFin`.`DT_MOV`, '%d/%m/%Y') AS 'Data do Pagamento',       
       ROUND((`ParcelaDocFin`.`VLR_QUITADO` * 0.39), 2) AS 'Comissao Socio1',
       ROUND((`ParcelaDocFin`.`VLR_QUITADO` * 0.0915), 2) AS 'Comissao Socio2',
       ROUND((`ParcelaDocFin`.`VLR_QUITADO` * 0.0915), 2) AS 'Comissao Socio3',
       ROUND((`ParcelaDocFin`.`VLR_QUITADO` * 0.427), 2) AS 'Comissao Socio4'       
FROM `DocFin_DocVenda`, `ParcelaDocFin`, `Venda`, `Cliente`
WHERE `DocFin_DocVenda`.`COD_DOC_FIN` = `ParcelaDocFin`.`COD_DOC_FIN`
AND `DocFin_DocVenda`.`COD_VENDA` = `Venda`.`COD_VENDA`
AND `Cliente`.`ID` = `Venda`.`COD_CLIENTE`
AND `ParcelaDocFin`.`DT_MOV` BETWEEN '2021-10-01' AND '2021-10-31'
ORDER BY `ParcelaDocFin`.`DT_MOV`;


SELECT `venda`.`COD_CONTROLE_VENDA` AS 'Codigo de Controle',
       `cliente`.`NOME` AS 'Nome do Cliente',
       `venda`.`VLR_TOTAL` AS 'Valor do Contrato',
       `parceladocfin`.`NUM_PARCELA` AS 'Origem do Pagamento',
       `parceladocfin`.`VLR_PARCELA` AS 'Valor da Parcela',
       `parceladocfin`.`DT_VENC` AS 'Data de Vencimento'
FROM `docfin_docvenda`, `parceladocfin`, `venda`, `cliente`
WHERE `docfin_docvenda`.`COD_DOC_FIN` = `parceladocfin`.`COD_DOC_FIN`
AND `docfin_docvenda`.`COD_VENDA` = `venda`.`COD_VENDA`
AND `cliente`.`ID` = `venda`.`COD_CLIENTE`
AND `parceladocfin`.`FORMA_PGTO` = 'Em Aberto'
ORDER BY `cliente`.`NOME`, `parceladocfin`.`NUM_PARCELA`;


SELECT *, (CURDATE() - INTERVAL 90 DAY) AS diff
FROM `parceladocfin`
WHERE `parceladocfin`.`FORMA_PGTO` = 'Em Aberto'
AND `parceladocfin`.`DT_VENC` <= CURDATE() - INTERVAL 90 DAY
GROUP BY `parceladocfin`.`COD_DOC_FIN`
ORDER BY `parceladocfin`.`DT_VENC` DESC;
