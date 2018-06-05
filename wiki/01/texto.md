# 1. Comece Aqui 

## 1.1 Sobre Este Tutorial

Esse Tutorial apresenta uma solu��o completa de implementa��o de um Sistema de Banco de Dados Espaciais em Sistema Gerenciador de Banco de Dados Objeto-Relacional e est� dividido em duas partes:

Parte I:

1. **Introdu��o ao PostGIS**

Parte II:

1. **Modelagem Conceitual**
2. **Cria��o de Esquema L�gico**
3. **Implementa��o F�sica**

A parte I desse tutorial foi baseado no workshop [Introduction to PostGIS](http://workshops.boundlessgeo.com/postgis-intro/) da empresa [Boundless](https://boundlessgeo.com/) e est� de acordo com a Licen�a Creative Commons Atribui��o-N�oComercial-CompartilhaIgual 4.0 Internacional.

## 1.2 Como usar esse tutorial 

### 1.2.1. Instru��es

As instru��es s�o indicadas pelo tipo de nota��o abaixo:

Exemplo:

>Clique no bot�o _Next_ para continuar.

### 1.2.2. C�digo

Instru��es em SQL ou PLPGSQL s�o apresentadas em uma caixa como no formato abaixo:

    SELECT postgis_full_version();

Este exemplo pode ser executado a partir de uma janela de consulta ou por meio de linhas de comando.

### 1.2.3. Nota��es

Nota��es s�o utilizadas para fornecer informa��es que s�o �teis, mas n�o s�o cr�ticas para o entendimento do t�pico.

Exemplo:  

    Nota��o:
    
    Aqui est� o exemplo de uma nota��o que � �til, por�m n�o � essencial ao entendimento do
    processo.

### 1.2.4. Fun��es

As fun��es s�o definidas a partir de fonte de texto em negrito.

Exemplo:

>**ST_Touches(geometry A, geometry B)** returns TRUE if either of the geometries� boundaries intersect

### 1.2.5. Arquivos, Esquemas, Tabelas e Colunas

Nomes de Arquivos, Esquemas, Tabelas e Colunas s�o representados por uma caixa envolt�ria ao nome como no exemplo abaixo:

>Selecione o atributo `nome` na tabela `municipio`.

### 1.2.6. Menus, Submenus, Bot�es e Outros Elementos

Menus/submenus, bot�es e outros elementos de interface gr�fica como campos ou check box s�o apresentados na formata��o de texto it�lico.

Exemplo:

>Clique em _Arquivo > Novo_.

>Marque a op��o com a palavra _Confirma_

## 1.3 Requisitos Tecnol�gicos

Postgresql 9.0+

PostGIS 2.0+

QGIS 2.0+