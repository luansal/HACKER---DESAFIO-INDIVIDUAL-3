# HACKER---DESAFIO-INDIVIDUAL-3


\\ criando a table PRODUTOS


CREATE SCHEMA `projeto` ;

CREATE TABLE `projeto`.`produtos` (
  	`id` INT NOT NULL AUTO_INCREMENT,
 	 `descriçao` VARCHAR(45) NOT NULL,
 	 `preço` DECIMAL(10) NOT NULL,
 	 `disponibilidade` VARCHAR(4) NOT NULL,
 	 `destaque` VARCHAR(4) NOT NULL,
 	 `id_departamento` INT NOT NULL,
PRIMARY KEY (`id`));

	ALTER TABLE `projeto`.`produtos` 
	DROP PRIMARY KEY,
	ADD PRIMARY KEY (`id`, `id_departamento`);
	;

\\ criando a table DEPARTAMENTO com foreing key em PRODUTOS

CREATE TABLE `projeto`.`departamento` (
  	`id_departamento` INT NOT NULL,
  	`nome` VARCHAR(45) NOT NULL,
  	  INDEX `departamento_idx` (`id_departamento` ASC) VISIBLE,
  	  CONSTRAINT `departamento`
  	  FOREIGN KEY (`id_departamento`)
  	  REFERENCES `projeto`.`produtos` (`id_departamento`)
  	  ON DELETE NO ACTION
  	  ON UPDATE NO ACTION);


\\ criando a table CLIENTES 

 CREATE TABLE `projeto`.`clientes` (
  	`nome` VARCHAR(30) NOT NULL,
  	`email` VARCHAR(45) NOT NULL,
  	`whatsapp` VARCHAR(15) NOT NULL,
  	`senha` VARCHAR(20) NOT NULL,
  	`clientescol1` VARCHAR(45) NULL,
 PRIMARY KEY (`nome`));

	ALTER TABLE `projeto`.`clientes` 
	ADD COLUMN `clientescol` VARCHAR(45) NULL AFTER `id`,
	CHANGE COLUMN `clientescol1` `id` INT NOT NULL AUTO_INCREMENT ,
	DROP PRIMARY KEY,
	ADD PRIMARY KEY (`id`);
	;

\\ criando a table endereço

CREATE TABLE `projeto`.`endereço` (
  `id` INT NOT NULL,
  `logradouro` VARCHAR(45) NOT NULL,
  `número` INT NOT NULL,
  `complemento` VARCHAR(45) NOT NULL,
  `cep` VARCHAR(45) NOT NULL,
  `bairro` VARCHAR(45) NOT NULL,
  `cidade` VARCHAR(45) NOT NULL,
  `estado` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`));

\\ ASSOCIANDO UM CLIENTE A UM ENDEREÇO ATRAVÉS DE FOREING KEY

ALTER TABLE `projeto`.`clientes` 
DROP FOREIGN KEY `endereço_cliente`;
ALTER TABLE `projeto`.`clientes` 
DROP COLUMN `clientescol1`,
DROP COLUMN `clientescol`,
CHANGE COLUMN `id` `id` INT NOT NULL ,
CHANGE COLUMN `endereço_id` `endereço_id` INT NOT NULL AUTO_INCREMENT ,
DROP PRIMARY KEY,
ADD PRIMARY KEY (`endereço_id`, `id`);
;
ALTER TABLE `projeto`.`clientes` 
ADD CONSTRAINT `endereço_cliente`
  FOREIGN KEY (`endereço_id`)
  REFERENCES `projeto`.`endereço` (`id`);



\\ criando a table PEDIDOS	

CREATE TABLE `projeto`.`pedidos` (
  `id` INT NOT NULL,
  `valor_total` DECIMAL(10) NOT NULL,
  `data` DATETIME NOT NULL,
  `status` INT NOT NULL,
  PRIMARY KEY (`id`, `status`));

\\ INSERÇÃO DE DADOS NAS TABLES

INSERT INTO `projeto`.`endereço` (`id`, `logradouro`, `número`, `complemento`, `cep`, `bairro`, `cidade`, `estado`) VALUES ('2', 'rua ttt ttt ttt', '40', 'atc', '550055', 'zona sul', 'são cristóvão', 'rio de janeiro');

INSERT INTO `projeto`.`clientes` (`nome`, `email`, `whatsapp`, `senha`, `id`, `endereço_id`) VALUES ('luis', 'asdf@gmail.com', '211212', '14242', '2', '2');

INSERT INTO `projeto`.`produtos` (`descriçao`, `preço`, `disponibilidade`, `destaque`, `id_departamento`) VALUES ('celular', '1000', 'sim', 'não', '2');

INSERT INTO `projeto`.`pedidos` (`id`, `valor_total`, `data`, `status`) VALUES ('2', '2500', '2021/04/05', '2');


\\ realizando contagem ou totalização 
\\ foi inserido alguns pedidos na tabela pedidos para realizar tal operação


INSERT INTO `projeto`.`pedidos` (`id`, `valor_total`, `data`, `status`) VALUES ('3', '1500', '2022/05/04', '1');
INSERT INTO `projeto`.`pedidos` (`id`, `valor_total`, `data`, `status`) VALUES ('4', '985', '2021-04-05 00:00:00', '3');
INSERT INTO `projeto`.`pedidos` (`id`, `valor_total`, `data`, `status`) VALUES ('6', '1800', '2021-04-05 00:00:00', '5');
INSERT INTO `projeto`.`pedidos` (`id`, `valor_total`, `data`, `status`) VALUES ('7', '555', '2021-04-05 00:00:00', '1');
INSERT INTO `projeto`.`pedidos` (`id`, `valor_total`, `data`, `status`) VALUES ('9', '455', '2021-04-05 00:00:00', '3');

SELECT count(1) FROM projeto.pedidos;

\\ junção da tabela clientes com a tabela endereços


SELECT * FROM projeto.clientes

inner join clientes on clientes.endereço_id = endereço.id


\\ junção de duas tabelas 

SELECT valor_total FROM projeto.pedidos

inner join pedidos on pedidos.id = produtos.pedidos.id
inner join pedidos on pedidos.id = clientes.pedidos.id


\\ junção de tabelas com agrupamentos


SELECT SUM(VALOR_TOTAL) AS SOMA_TOTAL FROM pedidos
inner join pedidos on pedidos.id = produtos.pedidos.id
group by cliente.id
