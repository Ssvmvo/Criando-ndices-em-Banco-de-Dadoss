# Criando-ndices-em-Banco-de-Dadoss
Criação de índices para consultas para o cenário de company com as perguntas (queries sql) para recuperação de informações. Sendo assim, dentro do script será criado os índices com base na consulta SQL.    O que será levado em consideração para criação dos índices?   Quais os dados mais acessados   Quais os dados mais relevantes no contexto 

Consultas mais frequentes: Quais colunas estão sendo usadas frequentemente em filtros (WHERE) ou em JOINs.
Colunas utilizadas em ORDER BY e GROUP BY: Índices podem acelerar operações de ordenação e agrupamento.
Tipo de índice: Dependendo da cardinalidade e tipo de dados, diferentes índices podem ser aplicados (BTREE, HASH, etc.).
Impacto no desempenho de inserção e atualização: Embora índices acelerem leituras, eles podem impactar negativamente operações de escrita.
Queries para o Cenário Company
Departamento com maior número de pessoas

sql
Copiar código
SELECT departamento_id, COUNT(*) AS total_empregados
FROM empregados
GROUP BY departamento_id
ORDER BY total_empregados DESC
LIMIT 1;
Departamentos por cidade

sql
Copiar código
SELECT cidade, GROUP_CONCAT(departamento_nome) AS departamentos
FROM departamentos
GROUP BY cidade;
Relação de empregados por departamento

sql
Copiar código
SELECT departamento_nome, GROUP_CONCAT(empregado_nome) AS empregados
FROM empregados
JOIN departamentos ON empregados.departamento_id = departamentos.id
GROUP BY departamento_nome;
Índices Criados
Para a query 1 (Departamento com maior número de pessoas):

sql
Copiar código
CREATE INDEX idx_departamento_empregados ON empregados(departamento_id);
Motivo: O índice na coluna departamento_id otimiza a contagem de empregados por departamento.

Para a query 2 (Departamentos por cidade):

sql
Copiar código
CREATE INDEX idx_cidade_departamentos ON departamentos(cidade);
Motivo: Como a consulta agrupa por cidade, o índice na coluna cidade agiliza a recuperação dos dados.

Para a query 3 (Relação de empregados por departamento):

sql
Copiar código
CREATE INDEX idx_empregados_departamentos ON empregados(departamento_id);
Motivo: O índice na coluna departamento_id melhora o desempenho na junção entre as tabelas empregados e departamentos.

Parte 2: Procedures para Manipulação de Dados
Você pode criar uma procedure genérica que recebe uma variável de controle para determinar a operação (inserção, atualização ou remoção). Abaixo está um exemplo básico de como você pode estruturar a procedure para manipulação de dados.

Procedure para Universidade (Exemplo)
sql
Copiar código
DELIMITER //

CREATE PROCEDURE sp_gerenciar_universidade(
    IN operacao INT,
    IN universidade_id INT,
    IN nome VARCHAR(100),
    IN cidade VARCHAR(100),
    IN nova_cidade VARCHAR(100)
)
BEGIN
    CASE operacao
        WHEN 1 THEN -- Inserir
            INSERT INTO universidades (nome, cidade) 
            VALUES (nome, cidade);
        WHEN 2 THEN -- Atualizar
            UPDATE universidades 
            SET cidade = nova_cidade
            WHERE id = universidade_id;
        WHEN 3 THEN -- Deletar
            DELETE FROM universidades 
            WHERE id = universidade_id;
    END CASE;
END //

DELIMITER ;
Procedure para E-commerce (Exemplo)
sql
Copiar código
DELIMITER //

CREATE PROCEDURE sp_gerenciar_ecommerce(
    IN operacao INT,
    IN produto_id INT,
    IN nome_produto VARCHAR(100),
    IN preco DECIMAL(10, 2),
    IN novo_preco DECIMAL(10, 2)
)
BEGIN
    CASE operacao
        WHEN 1 THEN -- Inserir
            INSERT INTO produtos (nome_produto, preco) 
            VALUES (nome_produto, preco);
        WHEN 2 THEN -- Atualizar
            UPDATE produtos 
            SET preco = novo_preco
            WHERE id = produto_id;
        WHEN 3 THEN -- Deletar
            DELETE FROM produtos 
            WHERE id = produto_id;
    END CASE;
END //

DELIMITER ;
README Explicativo (Modelo)
markdown
Copiar código
# Projeto: Criação de Índices e Procedures em Banco de Dados

## Descrição

Este projeto tem como objetivo otimizar consultas de banco de dados através da criação de índices eficientes, além de implementar procedures para manipulação de dados em cenários de universidade e e-commerce. As principais operações abordadas incluem inserção, atualização e remoção de dados, utilizando estruturas condicionais para controlar a ação executada pela procedure.

## Índices Criados

1. **`idx_departamento_empregados`**: Criado na tabela `empregados` na coluna `departamento_id` para otimizar a consulta que busca o departamento com maior número de empregados.
2. **`idx_cidade_departamentos`**: Criado na tabela `departamentos` na coluna `cidade` para agilizar a consulta que lista departamentos por cidade.
3. **`idx_empregados_departamentos`**: Criado na tabela `empregados` na coluna `departamento_id` para melhorar a performance na junção entre as tabelas `empregados` e `departamentos`.

## Procedures Criadas

- `sp_gerenciar_universidade`: Procedure responsável por inserir, atualizar ou deletar dados da tabela `universidades`, dependendo da variável de controle.
- `sp_gerenciar_ecommerce`: Procedure que permite gerenciar produtos (inserção, atualização e remoção) n
