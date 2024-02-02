# Estudo de caso
## O caso Bikeshop

### Empresa: BikeShop
#### Visão Geral:
A BikeShop é uma empresa especializada na venda de bicicletas e acessórios relacionados. 
Localizada em uma área urbana movimentada de Uberlândia, Minas Gerais, a empresa tem 
como objetivo oferecer uma variedade de bicicletas de alta qualidade para ciclistas de todos os 
níveis, desde iniciantes até ciclistas experientes e entusiastas.

#### Desafio:
A BikeShop está crescendo rapidamente e enfrenta desafios no gerenciamento eficiente de seu 
inventário, clientes e vendas. Atualmente, eles estão registrando essas informações 
manualmente ou usando planilhas eletrônicas, o que se tornou ineficiente e propenso a erros. 
Eles reconhecem a necessidade de um sistema de banco de dados centralizado que possa 
armazenar e gerenciar essas informações de forma mais eficaz.

#### Objetivos do Sistema de Banco de Dados:
Gerenciar o inventário de bicicletas e acessórios, incluindo detalhes como modelo, marca, 
quantidade em estoque, preço de venda e fornecedor.

Manter um registro centralizado de clientes, incluindo informações como nome, endereço, 
número de telefone, endereço de e-mail e histórico de compras.

Registrar e acompanhar as vendas de bicicletas e acessórios, incluindo detalhes como data da 
venda, produtos vendidos, preço de venda, método de pagamento e vendedor responsável.

#### Requisitos Funcionais do Sistema de Banco de Dados:
Capacidade de adicionar, atualizar e excluir itens do inventário, bem como verificar a 
disponibilidade de produtos em tempo real.

Capacidade de adicionar novos clientes, atualizar informações existentes e manter um histórico 
de suas compras anteriores.

Funcionalidade para registrar novas vendas, incluindo a associação dos produtos vendidos aos 
clientes correspondentes e a geração de recibos.

Recursos de segurança para proteger os dados do cliente e do inventário contra acesso não 
autorizado.

Capacidade de gerar relatórios de vendas, análises de estoque e dados do cliente para ajudar 
na tomada de decisões comerciais.

#### Abordagem Proposta:
A BikeShop planeja desenvolver um sistema de banco de dados personalizado usando 
tecnologias modernas de banco de dados, como MySQL ou PostgreSQL. Eles planejam 
colaborar com desenvolvedores de software especializados para projetar e implementar o 
sistema de acordo com seus requisitos específicos. O sistema será acessado por funcionários 
autorizados por meio de uma interface de usuário intuitiva, onde poderão realizar todas as 
operações necessárias de forma eficiente.

#### Benefícios Esperados:
Melhoria na eficiência operacional, permitindo que a BikeShop gerencie seu inventário, clientes 
e vendas de forma mais rápida e precisa.

Maior satisfação do cliente, oferecendo um serviço mais personalizado e mantendo um 
histórico detalhado das interações anteriores.

Melhoria na tomada de decisões comerciais com base em relatórios e análises de dados 
precisos e atualizados.

Com um sistema de banco de dados eficiente e bem projetado, a BikeShop está confiante de 
que poderá atender às demandas de seus clientes de maneira mais eficaz e continuar 
prosperando no mercado de bicicletas.

### Modelo conceitual do estudo de caso

!["modelo conceitual"](modelo-conceitual-bikeshop.png)

### Modelo Lógico

!["modelo Lógico"](modelo-bikeshop.png)

### Tabela de entidades feito no Excel

!["tabela de entidade"](modelo-excel.png)

### Código SQL para o modelo físico do banco de dados
___

```sql
-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
DROP SCHEMA IF EXISTS `mydb` ;

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `mydb` DEFAULT CHARACTER SET utf8 ;
SHOW WARNINGS;
USE `mydb` ;

-- -----------------------------------------------------
-- Table `mydb`.`Fornecedores`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Fornecedores` (
  `id_fornecedor` INT NOT NULL AUTO_INCREMENT,
  `nome_fornecedor` VARCHAR(30) NOT NULL,
  `endereco_fornecedor` VARCHAR(150) NOT NULL,
  `email_fornecedor` VARCHAR(50) NOT NULL,
  `numero_telefone_fornecedor` VARCHAR(15) NOT NULL,
  PRIMARY KEY (`id_fornecedor`),
  UNIQUE INDEX `email_fornecedor_UNIQUE` (`email_fornecedor` ASC) VISIBLE)
ENGINE = InnoDB;

SHOW WARNINGS;

-- -----------------------------------------------------
-- Table `mydb`.`Inventario`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Inventario` (
  `idInventario` INT NOT NULL AUTO_INCREMENT,
  `modelo` VARCHAR(50) NOT NULL,
  `marca` VARCHAR(20) NOT NULL,
  `quantidade` INT NOT NULL,
  `preco` DECIMAL(10,2) NOT NULL,
  `id_fornecedor` INT NOT NULL,
  PRIMARY KEY (`idInventario`, `id_fornecedor`),
  INDEX `fk_Inventario_Fornecedores_idx` (`id_fornecedor` ASC) VISIBLE,
  CONSTRAINT `fk_Inventario_Fornecedores`
    FOREIGN KEY (`id_fornecedor`)
    REFERENCES `mydb`.`Fornecedores` (`id_fornecedor`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

SHOW WARNINGS;

-- -----------------------------------------------------
-- Table `mydb`.`CLientes`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`CLientes` (
  `id_cliente` INT NOT NULL AUTO_INCREMENT,
  `nome` VARCHAR(30) NOT NULL,
  `endereco` VARCHAR(150) NOT NULL,
  `numero_telefone` VARCHAR(15) NOT NULL,
  `email_cliente` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`id_cliente`),
  UNIQUE INDEX `email_cliente_UNIQUE` (`email_cliente` ASC) VISIBLE)
ENGINE = InnoDB;

SHOW WARNINGS;

-- -----------------------------------------------------
-- Table `mydb`.`Funcionarios`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Funcionarios` (
  `id_funcionario` INT NOT NULL AUTO_INCREMENT,
  `nome_funcionario` VARCHAR(30) NOT NULL,
  `cargo` VARCHAR(30) NOT NULL,
  `salario` DECIMAL(10,2) NOT NULL,
  `data_admissao` DATE NOT NULL,
  PRIMARY KEY (`id_funcionario`))
ENGINE = InnoDB;

SHOW WARNINGS;

-- -----------------------------------------------------
-- Table `mydb`.`Vendedores`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Vendedores` (
  `id_vendedor` INT NOT NULL AUTO_INCREMENT,
  `nome_vendedor` VARCHAR(30) NOT NULL,
  `id_funcionario` INT NOT NULL,
  PRIMARY KEY (`id_vendedor`, `id_funcionario`),
  INDEX `fk_Vendedores_Funcionarios1_idx` (`id_funcionario` ASC) VISIBLE,
  UNIQUE INDEX `id_funcionario_UNIQUE` (`id_funcionario` ASC) VISIBLE,
  CONSTRAINT `fk_Vendedores_Funcionarios1`
    FOREIGN KEY (`id_funcionario`)
    REFERENCES `mydb`.`Funcionarios` (`id_funcionario`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

SHOW WARNINGS;

-- -----------------------------------------------------
-- Table `mydb`.`Vendas`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Vendas` (
  `id_venda` INT NOT NULL AUTO_INCREMENT,
  `data_venda` DATETIME NOT NULL,
  `quantidade_venda` INT NOT NULL,
  `preco_total` DECIMAL(10,2) NOT NULL,
  `metodo_pagamento` VARCHAR(30) NOT NULL,
  `id_cliente` INT NOT NULL,
  `id_vendedor` INT NOT NULL,
  PRIMARY KEY (`id_venda`, `id_cliente`, `id_vendedor`),
  INDEX `fk_Vendas_CLientes1_idx` (`id_cliente` ASC) VISIBLE,
  INDEX `fk_Vendas_Vendedores1_idx` (`id_vendedor` ASC) VISIBLE,
  CONSTRAINT `fk_Vendas_CLientes1`
    FOREIGN KEY (`id_cliente`)
    REFERENCES `mydb`.`CLientes` (`id_cliente`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Vendas_Vendedores1`
    FOREIGN KEY (`id_vendedor`)
    REFERENCES `mydb`.`Vendedores` (`id_vendedor`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

SHOW WARNINGS;

-- -----------------------------------------------------
-- Table `mydb`.`Vendas_has_Inventario`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Vendas_has_Inventario` (
  `Vendas_id_venda` INT NOT NULL,
  `Inventario_idInventario` INT NOT NULL,
  PRIMARY KEY (`Vendas_id_venda`, `Inventario_idInventario`),
  INDEX `fk_Vendas_has_Inventario_Inventario1_idx` (`Inventario_idInventario` ASC) VISIBLE,
  INDEX `fk_Vendas_has_Inventario_Vendas1_idx` (`Vendas_id_venda` ASC) VISIBLE,
  CONSTRAINT `fk_Vendas_has_Inventario_Vendas1`
    FOREIGN KEY (`Vendas_id_venda`)
    REFERENCES `mydb`.`Vendas` (`id_venda`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Vendas_has_Inventario_Inventario1`
    FOREIGN KEY (`Inventario_idInventario`)
    REFERENCES `mydb`.`Inventario` (`idInventario`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

SHOW WARNINGS;

SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

```