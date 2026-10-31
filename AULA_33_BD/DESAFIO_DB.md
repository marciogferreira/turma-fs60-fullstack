# DESAFIO PRÁTICO: SISTEMA DE CADASTRO DE CLIENTES COM SQL

**Disciplina:** Banco de Dados  
**Tecnologia:** PostgreSQL  
**Tema:** DDL, DML e DQL

## 1. Objetivo

Desenvolver a estrutura de um cadastro de clientes utilizando SQL, aplicando comandos para criação de tabelas, inserção, consulta, atualização e exclusão de registros.

O objetivo é compreender como funciona a manipulação de dados em um banco de dados relacional.

## 2. Cenário

Uma empresa precisa desenvolver um sistema para gerenciar seus clientes. Para isso, você deverá criar uma tabela no banco de dados PostgreSQL e realizar as operações necessárias para manter as informações atualizadas.

Sua missão é desenvolver o cadastro utilizando exclusivamente comandos SQL.

## 3. Atividade 1 — Criar a tabela

Crie uma tabela chamada `clientes` contendo os seguintes campos:

| Campo | Tipo de dado | Regra |
|---|---|---|
| id | SERIAL | Chave primária |
| nome | VARCHAR(100) | Obrigatório |
| email | VARCHAR(100) | Obrigatório |
| telefone | VARCHAR(20) | Opcional |
| cidade | VARCHAR(80) | Obrigatório |
| criado_em | TIMESTAMP | Data de criação |
| atualizado_em | TIMESTAMP | Data da última atualização |

**Requisitos:**
- Utilize `CREATE TABLE`.
- Defina o campo `id` como chave primária.
- Utilize `NOT NULL` nos campos obrigatórios.
- Configure `criado_em` e `atualizado_em` com valor padrão `CURRENT_TIMESTAMP`.

## 4. Atividade 2 — Inserir clientes

Insira **5 clientes fictícios** na tabela.

Cada cliente deverá possuir:
- Nome completo;
- E-mail;
- Telefone;
- Cidade.

**Requisitos:**
- Utilize o comando `INSERT INTO`.
- Insira os cinco clientes.
- Verifique se as datas de criação e atualização foram preenchidas automaticamente.

## 5. Atividade 3 — Consultar clientes

Realize as seguintes consultas:

1. Liste todos os clientes cadastrados.
2. Consulte apenas o nome e o e-mail dos clientes.
3. Consulte os clientes que moram em uma cidade específica.
4. Consulte um cliente utilizando seu ID.
5. Liste os clientes em ordem alfabética pelo nome.
6. Consulte o nome, a cidade e a data de criação de todos os clientes.

**Requisitos:** utilize `SELECT`, `WHERE` e `ORDER BY`.

## 6. Atividade 4 — Atualizar clientes

Realize as seguintes operações:

1. Atualize o telefone de um cliente.
2. Atualize o e-mail de outro cliente.
3. Altere a cidade de um cliente específico.
4. Atualize o nome de um cliente.

**Requisitos:**
- Utilize o comando `UPDATE`.
- Utilize a cláusula `WHERE` para identificar o registro.
- Atualize também o campo `atualizado_em` utilizando `CURRENT_TIMESTAMP`.

**Importante:** o campo `criado_em` não deverá ser alterado durante as atualizações.

## 7. Atividade 5 — Excluir clientes

Realize as seguintes operações:

1. Exclua um cliente específico utilizando seu ID.
2. Consulte todos os clientes novamente.
3. Verifique se o cliente foi realmente removido.

**Requisitos:**
- Utilize o comando `DELETE FROM`.
- Utilize a cláusula `WHERE`.

**Atenção:** sem a cláusula `WHERE`, todos os registros da tabela poderão ser excluídos.

## 8. Atividade 6 — Desafio extra

Para demonstrar seus conhecimentos, execute as seguintes tarefas:

1. Insira mais dois clientes.
2. Consulte todos os clientes cujo nome começa com a letra A.
3. Consulte os clientes de uma cidade escolhida por você.
4. Consulte os clientes cadastrados nos últimos dias.
5. Atualize o telefone de um cliente e verifique a alteração da data `atualizado_em`.
6. Consulte todos os registros mostrando `criado_em` e `atualizado_em`.
7. Exclua um cliente e apresente o resultado da consulta após a exclusão.

## 9. Entrega da atividade

O aluno deverá entregar um arquivo `.sql` contendo:

- Script de criação da tabela;
- Inserção dos cinco clientes;
- Consultas solicitadas;
- Atualizações dos registros;
- Exclusão de clientes;
- Consultas de validação dos resultados.

## 10. Conteúdos avaliados

| Categoria | Comando | Finalidade |
|---|---|---|
| DDL | CREATE TABLE | Criar tabela |
| DML | INSERT INTO | Inserir registros |
| DQL | SELECT | Consultar registros |
| DML | UPDATE | Atualizar registros |
| DML | DELETE | Excluir registros |
| SQL | CURRENT_TIMESTAMP | Trabalhar com data e hora |
| SQL | WHERE | Filtrar registros |
| SQL | ORDER BY | Ordenar resultados |
| SQL | LIKE | Pesquisar por padrões |

**Critérios de avaliação:**
- Criação correta da estrutura da tabela;
- Inserção dos dados;
- Utilização correta dos comandos SQL;
- Atualização correta dos campos de data;
- Consultas e filtros funcionando;
- Organização e legibilidade do script.

**Bom trabalho!**
<!-- 
A gestora Ana precisa de um relatório para analisar os produtos cadastrados no sistema, considerando seus preços de venda e de custo.

O relatório deverá apresentar o nome do produto, o preço de venda e o preço de custo, exibindo exclusivamente os produtos cujo preço de venda seja superior a R$ 300,00 e inferior a R$ 700,00.

O objetivo é facilitar a análise dos produtos dentro dessa faixa de preço, auxiliando a gestora na tomada de decisões comerciais.
 -->