Terceiro Trabalho - Programação Web
=========

> Informações adicionais:
Site da disciplina - http://cursos.meslin.com.br/home/jsp

> Configuração do Eclipse:
http://www.slideshare.net/loianeg/using-the-egit-eclipse-plugin-with-git-hub-2578587#

> Instrução Clonar Projeto existente
Inserir instruções

Script para criação do banco no SGBD MySQL:
=========

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='TRADITIONAL,ALLOW_INVALID_DATES';

CREATE SCHEMA IF NOT EXISTS `Reserva` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci ;
USE `Reserva` ;

-- Table `Reserva`.`usuario`

CREATE  TABLE IF NOT EXISTS `Reserva`.`usuario` (
  `login` VARCHAR(20) NOT NULL ,
  `nome` VARCHAR(100) NOT NULL ,
  `email` VARCHAR(100) NOT NULL ,
  `senha` VARCHAR(20) NOT NULL ,
  `sexo` CHAR NOT NULL ,
  `admin` TINYINT NOT NULL ,
  PRIMARY KEY (`login`) )
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8
COLLATE = utf8_general_ci;

-- Table `Reserva`.`tipo_unidade`

CREATE  TABLE IF NOT EXISTS `Reserva`.`tipo_unidade` (
  `id` INT NOT NULL AUTO_INCREMENT ,
  `tipo` VARCHAR(45) NOT NULL ,
  PRIMARY KEY (`id`) )
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8
COLLATE = utf8_general_ci;

CREATE UNIQUE INDEX `unq_tipo_unidade` ON `Reserva`.`tipo_unidade` (`tipo` ASC) ;

-- Table `Reserva`.`unidade`

CREATE  TABLE IF NOT EXISTS `Reserva`.`unidade` (
  `id` INT NOT NULL AUTO_INCREMENT ,
  `nome` VARCHAR(90) NOT NULL ,
  `id_tipo_unidade` INT NOT NULL ,
  PRIMARY KEY (`id`) ,
  CONSTRAINT `fk_tipo_unidade`
    FOREIGN KEY (`id_tipo_unidade` )
    REFERENCES `Reserva`.`tipo_unidade` (`id` )
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8
COLLATE = utf8_general_ci;

CREATE UNIQUE INDEX `unq_nome_unidade` ON `Reserva`.`unidade` (`nome` ASC) ;

CREATE INDEX `fk_tipo_unidade` ON `Reserva`.`unidade` (`id_tipo_unidade` ASC) ;

-- Table `Reserva`.`reserva`

CREATE  TABLE IF NOT EXISTS `Reserva`.`reserva` (
  `id` INT NOT NULL AUTO_INCREMENT ,
  `login_usuario` VARCHAR(20) NOT NULL ,
  `id_unidade` INT NOT NULL ,
  `dt_reserva` DATETIME NOT NULL ,
  `dt_avaliacao` DATETIME NULL DEFAULT NULL ,
  `login_usuario_admin` VARCHAR(20) NULL DEFAULT NULL ,
  `aceita` TINYINT NULL DEFAULT NULL ,
  `justificativa` VARCHAR(255) NULL DEFAULT NULL ,
  PRIMARY KEY (`id`) ,
  CONSTRAINT `fk_login_reserva`
    FOREIGN KEY (`login_usuario` )
    REFERENCES `Reserva`.`usuario` (`login` )
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_unidade_reserva`
    FOREIGN KEY (`id_unidade` )
    REFERENCES `Reserva`.`unidade` (`id` )
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_login_admin_reserva`
    FOREIGN KEY (`login_usuario_admin` )
    REFERENCES `Reserva`.`usuario` (`login` )
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8
COLLATE = utf8_general_ci;

CREATE INDEX `fk_login_reserva` ON `Reserva`.`reserva` (`login_usuario` ASC) ;

CREATE INDEX `fk_unidade_reserva` ON `Reserva`.`reserva` (`id_unidade` ASC) ;

CREATE UNIQUE INDEX `unq_login_unidade_data_reserva` ON `Reserva`.`reserva` (`login_usuario` ASC, `id_unidade` ASC, `dt_reserva` ASC) ;

CREATE INDEX `fk_login_admin_reserva` ON `Reserva`.`reserva` (`login_usuario_admin` ASC) ;

USE `Reserva` ;

SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
