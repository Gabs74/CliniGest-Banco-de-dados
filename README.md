[README_clinigest.md](https://github.com/user-attachments/files/28123571/README_clinigest.md)
# Clinigest - Sistema de Banco de Dados para Clínica Médica

## 1. Apresentação do projeto

O **Clinigest** é um projeto de banco de dados feito para organizar as informações básicas de uma clínica médica. O sistema foi pensado para controlar pacientes, médicos, especialidades, convênios, agendamentos, consultas, prontuários, exames, prescrições e pagamentos.

A ideia principal é mostrar como um banco de dados pode ajudar uma clínica a guardar informações importantes de forma organizada, evitando repetição de dados e facilitando consultas do dia a dia.

## 2. Tema escolhido

**Sistema de gerenciamento para clínica médica.**

## 3. Objetivo geral

Criar um banco de dados simples, organizado e funcional para uma clínica médica, usando comandos SQL para criar tabelas, inserir dados, consultar informações, atualizar registros e excluir dados quando necessário.

## 4. Tecnologias usadas

- MySQL
- MySQL Workbench
- SQL
- GitHub para armazenar o código e a documentação

## 5. Como executar o projeto

1. Abra o MySQL Workbench.
2. Crie uma nova aba de consulta.
3. Abra o arquivo `sql/clinigest.sql`.
4. Execute o script completo.
5. Depois da criação e dos inserts, execute as consultas por partes para tirar os prints de evidência.
6. Execute os comandos `DELETE` somente no final, depois dos prints principais.

## 6. Estrutura dos arquivos no GitHub

```text
clinigest/
├── README.md
├── sql/
│   └── clinigest.sql
├── imagens/
│   └── der_clinigest.png
├── docs/
│   ├── documentacao_clinigest.docx
│   └── documentacao_clinigest.pdf
└── apresentacao/
    └── apresentacao_clinigest.pptx
```

## 7. Tabelas do banco de dados

O banco possui 12 tabelas:

| Tabela | Função |
|---|---|
| `convenios` | Guarda os convênios aceitos pela clínica. |
| `pacientes` | Guarda os dados dos pacientes. |
| `especialidades` | Guarda as áreas médicas, como cardiologia e pediatria. |
| `medicos` | Guarda os dados dos médicos. |
| `medico_especialidades` | Liga médicos às suas especialidades. |
| `agendamentos` | Registra os horários marcados pelos pacientes. |
| `consultas` | Registra as consultas realizadas ou canceladas. |
| `prontuarios` | Guarda a evolução do atendimento médico. |
| `tipos_exames` | Guarda os tipos de exames e seus preços. |
| `exames` | Registra exames solicitados nas consultas. |
| `prescricoes` | Registra medicamentos e orientações dadas ao paciente. |
| `pagamentos` | Registra valores pagos, forma de pagamento e convênio usado. |

## 8. Regras de negócio

1. Um paciente pode estar ligado a um convênio, mas também pode não ter convênio.
2. Um médico pode ter uma ou mais especialidades.
3. Um paciente pode ter vários agendamentos.
4. Um agendamento pertence a um paciente e a um médico.
5. Uma consulta pode gerar prontuário, exames, prescrições e pagamento.
6. Um pagamento pode ser feito por dinheiro, cartão, pix ou convênio.
7. O CPF do paciente deve ser único.
8. O CRM do médico deve ser único.
9. O e-mail de pacientes, médicos e convênios foi definido como único para evitar duplicidade.
10. Os comandos de exclusão devem ser executados por último, para não atrapalhar os testes e os prints.

## 9. Normalização aplicada

### Primeira Forma Normal - 1FN

A 1FN foi aplicada porque os campos guardam apenas um valor por coluna. Por exemplo, o telefone do paciente fica em uma coluna própria, o CPF em outra e o e-mail em outra. Isso evita colocar várias informações juntas no mesmo campo.

### Segunda Forma Normal - 2FN

A 2FN foi aplicada principalmente na tabela `medico_especialidades`. Em vez de colocar várias especialidades dentro da tabela `medicos`, foi criada uma tabela separada para ligar médicos e especialidades.

Isso resolve o problema de repetição. Um médico pode ter mais de uma especialidade e uma especialidade pode pertencer a vários médicos. A tabela `medico_especialidades` guarda apenas essa ligação.

### Terceira Forma Normal - 3FN

A 3FN foi aplicada separando dados que não dependem diretamente da tabela principal. Por exemplo, o nome do convênio fica na tabela `convenios`, e na tabela `pacientes` fica apenas o `convenio_id`. Assim, se o nome do convênio mudar, a alteração é feita em um único lugar.

## 10. Explicação dos principais comandos

### CREATE DATABASE e CREATE TABLE

Esses comandos criam o banco de dados e as tabelas. Cada tabela possui uma chave primária, representada por campos como `paciente_id`, `medico_id`, `consulta_id` e outros. As chaves estrangeiras são usadas para ligar uma tabela a outra.

Exemplo:

```sql
CREATE DATABASE clinigest;
USE clinigest;
```

### INSERT

Os comandos `INSERT` foram usados para preencher o banco com dados de exemplo. Eles permitem testar as consultas e mostrar o funcionamento do sistema.

Exemplo:

```sql
INSERT INTO convenios (nome, telefone, email) VALUES
('Saude Total', '8132011001', 'contato@saudetotal.com');
```

Os inserts completos estão no arquivo `sql/clinigest.sql`.

## 11. Consultas, alterações e exclusões usadas no projeto

Abaixo estão as consultas e comandos usados para demonstrar o funcionamento do banco. Cada bloco mostra o comando SQL e o uso esperado dentro do sistema.

### 11.1 Consultas simples SELECT

Essas consultas mostram dados básicos das tabelas. Elas servem para listar pacientes, médicos, especialidades, consultas, exames, prescrições e pagamentos.

```sql
SELECT * FROM pacientes;
SELECT nome, cpf, telefone FROM pacientes;
SELECT * FROM medicos;
SELECT nome FROM especialidades;
SELECT * FROM convenios;
SELECT agendamento_id, data_hora, status FROM agendamentos;
SELECT consulta_id, data_hora, status FROM consultas;
SELECT prontuario_id, data_registro FROM prontuarios;
SELECT nome, preco FROM tipos_exames;
SELECT exame_id, data_solicitacao, resultado FROM exames;
SELECT medicamento, posologia FROM prescricoes;
SELECT pagamento_id, valor, forma_pagamento FROM pagamentos;
SELECT paciente_id, nome, email FROM pacientes ORDER BY nome;
SELECT medico_id, nome, crm FROM medicos ORDER BY nome;
SELECT tipo_exame_id, nome, preco FROM tipos_exames ORDER BY preco DESC;
```

**Como será usado:** essas consultas podem ser usadas em telas de listagem, como lista de pacientes, lista de médicos, lista de exames e tela financeira.

### 11.2 Consultas com filtro

Essas consultas buscam informações específicas. Elas usam `WHERE`, `LIKE`, `BETWEEN`, `IS NULL` e `IS NOT NULL`.

```sql
SELECT * FROM pacientes WHERE convenio_id = 2;
SELECT * FROM pacientes WHERE nome LIKE 'M%';
SELECT * FROM pacientes WHERE data_nascimento >= '1990-01-01';
SELECT * FROM medicos WHERE nome LIKE '%Silva%';
SELECT * FROM agendamentos WHERE status = 'confirmado';
SELECT * FROM agendamentos WHERE data_hora BETWEEN '2026-06-01' AND '2026-06-05';
SELECT * FROM consultas WHERE status = 'realizada';
SELECT * FROM consultas WHERE medico_id = 1;
SELECT * FROM tipos_exames WHERE preco > 100.00;
SELECT * FROM exames WHERE resultado IS NULL;
SELECT * FROM prescricoes WHERE medicamento LIKE '%Dipirona%';
SELECT * FROM pagamentos WHERE forma_pagamento = 'pix';
SELECT * FROM pagamentos WHERE convenio_id IS NOT NULL;
SELECT * FROM pagamentos WHERE valor BETWEEN 100.00 AND 200.00;
SELECT * FROM convenios WHERE nome LIKE '%Saude%';
```

**Como será usado:** essas consultas ajudam a procurar pacientes, filtrar consultas por status, buscar pagamentos por forma de pagamento e localizar exames sem resultado.

### 11.3 Consultas com JOIN

Os `JOINs` juntam informações de tabelas diferentes. No projeto, a maioria usa `INNER JOIN`, mas também foram usados `LEFT JOIN` e `RIGHT JOIN`.

#### Consulta 1 - Agendamentos com paciente e médico

```sql
SELECT a.agendamento_id, p.nome AS paciente, m.nome AS medico, a.data_hora, a.status
FROM agendamentos a
INNER JOIN pacientes p ON a.paciente_id = p.paciente_id
INNER JOIN medicos m ON a.medico_id = m.medico_id;
```

**Uso:** mostrar a agenda da clínica com o nome do paciente e do médico.

#### Consulta 2 - Consultas com paciente e médico

```sql
SELECT co.consulta_id, p.nome AS paciente, m.nome AS medico, co.data_hora, co.status
FROM consultas co
INNER JOIN pacientes p ON co.paciente_id = p.paciente_id
INNER JOIN medicos m ON co.medico_id = m.medico_id;
```

**Uso:** listar consultas realizadas ou canceladas com os nomes envolvidos.

#### Consulta 3 - Médicos e especialidades

```sql
SELECT m.nome AS medico, e.nome AS especialidade
FROM medicos m
INNER JOIN medico_especialidades me ON m.medico_id = me.medico_id
INNER JOIN especialidades e ON me.especialidade_id = e.especialidade_id;
```

**Uso:** mostrar qual especialidade cada médico atende.

#### Consulta 4 - Prontuários por paciente

```sql
SELECT pr.prontuario_id, p.nome AS paciente, pr.evolucao
FROM prontuarios pr
INNER JOIN consultas co ON pr.consulta_id = co.consulta_id
INNER JOIN pacientes p ON co.paciente_id = p.paciente_id;
```

**Uso:** consultar o histórico clínico de cada paciente.

#### Consulta 5 - Exames por paciente

```sql
SELECT ex.exame_id, p.nome AS paciente, te.nome AS exame, ex.resultado
FROM exames ex
INNER JOIN consultas co ON ex.consulta_id = co.consulta_id
INNER JOIN pacientes p ON co.paciente_id = p.paciente_id
INNER JOIN tipos_exames te ON ex.tipo_exame_id = te.tipo_exame_id;
```

**Uso:** listar exames solicitados e seus resultados.

#### Consulta 6 - Prescrições por paciente

```sql
SELECT pe.prescricao_id, p.nome AS paciente, pe.medicamento, pe.posologia
FROM prescricoes pe
INNER JOIN consultas co ON pe.consulta_id = co.consulta_id
INNER JOIN pacientes p ON co.paciente_id = p.paciente_id;
```

**Uso:** mostrar medicamentos prescritos para cada paciente.

#### Consulta 7 - Pagamentos por paciente

```sql
SELECT pg.pagamento_id, p.nome AS paciente, pg.valor, pg.forma_pagamento
FROM pagamentos pg
INNER JOIN pacientes p ON pg.paciente_id = p.paciente_id;
```

**Uso:** acompanhar os pagamentos da clínica.

#### Consulta 8 - Pacientes com ou sem convênio

```sql
SELECT p.nome AS paciente, c.nome AS convenio
FROM pacientes p
LEFT JOIN convenios c ON p.convenio_id = c.convenio_id;
```

**Uso:** mostrar todos os pacientes, mesmo os que não possuem convênio.

#### Consulta 9 - Pagamentos com convênio, quando existir

```sql
SELECT pg.pagamento_id, p.nome AS paciente, cv.nome AS convenio, pg.valor
FROM pagamentos pg
INNER JOIN pacientes p ON pg.paciente_id = p.paciente_id
LEFT JOIN convenios cv ON pg.convenio_id = cv.convenio_id;
```

**Uso:** mostrar pagamentos e identificar quando foram feitos por convênio.

#### Consulta 10 - Convênios e seus pacientes

```sql
SELECT c.nome AS convenio, p.nome AS paciente
FROM pacientes p
RIGHT JOIN convenios c ON p.convenio_id = c.convenio_id;
```

**Uso:** listar todos os convênios, mesmo se algum ainda não tiver paciente cadastrado.

### 11.4 Consultas com agregação

Essas consultas usam `COUNT`, `SUM`, `AVG` e `GROUP BY`. Elas servem para relatórios.

```sql
SELECT COUNT(*) AS total_pacientes FROM pacientes;
SELECT COUNT(*) AS total_medicos FROM medicos;
SELECT COUNT(*) AS total_consultas FROM consultas;
SELECT SUM(valor) AS total_pagamentos FROM pagamentos;
SELECT SUM(valor) AS total_pix FROM pagamentos WHERE forma_pagamento = 'pix';
SELECT SUM(preco) AS soma_precos_exames FROM tipos_exames;
SELECT AVG(valor) AS media_pagamentos FROM pagamentos;
SELECT AVG(preco) AS media_preco_exames FROM tipos_exames;
SELECT AVG(TIMESTAMPDIFF(YEAR, data_nascimento, CURDATE())) AS media_idade_pacientes FROM pacientes;
SELECT forma_pagamento, COUNT(*) AS quantidade, SUM(valor) AS total FROM pagamentos GROUP BY forma_pagamento;
SELECT status, COUNT(*) AS quantidade FROM agendamentos GROUP BY status;
SELECT medico_id, COUNT(*) AS total_consultas FROM consultas GROUP BY medico_id;
```

**Como será usado:** essas consultas podem gerar relatórios, como total de pacientes, total recebido, média de pagamentos e quantidade de consultas por médico.

### 11.5 UPDATE

Os comandos `UPDATE` alteram informações já cadastradas. Foram usados para simular correções de dados e mudanças de status.

```sql
UPDATE pacientes SET telefone = '81999990001' WHERE paciente_id = 1;
UPDATE pacientes SET endereco = 'Avenida Principal, 100, Olinda-PE' WHERE paciente_id = 2;
UPDATE medicos SET telefone = '81999990002' WHERE medico_id = 1;
UPDATE convenios SET telefone = '8133330001' WHERE convenio_id = 2;
UPDATE agendamentos SET status = 'confirmado' WHERE agendamento_id = 1;
UPDATE agendamentos SET status = 'cancelado' WHERE agendamento_id = 2;
UPDATE consultas SET status = 'realizada' WHERE consulta_id = 10;
UPDATE tipos_exames SET preco = 39.90 WHERE tipo_exame_id = 1;
UPDATE pagamentos SET valor = 120.00 WHERE pagamento_id = 1;
UPDATE prescricoes SET posologia = 'Tomar 1 comprimido a cada 8 horas.' WHERE prescricao_id = 1;
```

**Como será usado:** esses comandos representam situações reais, como atualizar telefone, corrigir endereço, confirmar agendamento, cancelar agendamento, alterar preço de exame e ajustar orientação de medicamento.

### 11.6 DELETE

Os comandos `DELETE` removem registros. No projeto, eles devem ser executados por último para não apagar dados antes dos testes.

```sql
DELETE FROM pagamentos WHERE pagamento_id = 50;
DELETE FROM pagamentos WHERE pagamento_id = 49;
DELETE FROM exames WHERE exame_id = 50;
DELETE FROM exames WHERE exame_id = 49;
DELETE FROM prescricoes WHERE prescricao_id = 50;
DELETE FROM prescricoes WHERE prescricao_id = 49;
DELETE FROM prontuarios WHERE prontuario_id = 50;
DELETE FROM prontuarios WHERE prontuario_id = 49;
DELETE FROM medico_especialidades WHERE medico_id = 20;
DELETE FROM medico_especialidades WHERE medico_id = 19;
```

**Como será usado:** esses comandos simulam a exclusão de dados cadastrados incorretamente ou registros de teste. Eles foram feitos em tabelas que não prejudicam a estrutura principal do banco.

## 12. Diferença entre INNER JOIN, LEFT JOIN e RIGHT JOIN no projeto

- `INNER JOIN`: mostra apenas os registros que possuem ligação nas duas tabelas.
- `LEFT JOIN`: mostra todos os registros da tabela da esquerda, mesmo que não exista ligação na tabela da direita.
- `RIGHT JOIN`: mostra todos os registros da tabela da direita, mesmo que não exista ligação na tabela da esquerda.

No Clinigest, o `INNER JOIN` foi usado na maioria das consultas porque normalmente queremos mostrar dados que possuem ligação real, como consulta com paciente e médico. O `LEFT JOIN` foi usado para mostrar pacientes mesmo sem convênio. O `RIGHT JOIN` foi usado para mostrar todos os convênios, mesmo os que não possuem pacientes.

## 13. Evidências para colocar na documentação

Para completar a entrega, recomenda-se tirar prints de:

1. DER do banco.
2. Execução do `CREATE DATABASE` e das tabelas.
3. Tabelas criadas no MySQL Workbench.
4. Inserts executados com sucesso.
5. Consulta simples SELECT.
6. Consulta com filtro.
7. Consulta com INNER JOIN.
8. Consulta com LEFT JOIN.
9. Consulta com RIGHT JOIN.
10. Consulta com agregação.
11. UPDATE executado.
12. DELETE executado.

## 14. Roteiro curto para apresentação

1. Apresentar o tema: sistema para clínica médica.
2. Explicar o problema: organizar pacientes, médicos, consultas, exames e pagamentos.
3. Mostrar o DER e explicar as tabelas principais.
4. Explicar a tabela `medico_especialidades`, que resolve a relação muitos para muitos entre médicos e especialidades.
5. Mostrar a normalização: 1FN, 2FN e 3FN.
6. Abrir o MySQL e mostrar o banco criado.
7. Executar uma consulta simples.
8. Executar uma consulta com filtro.
9. Executar um `INNER JOIN`.
10. Mostrar uma consulta com agregação.
11. Explicar UPDATE e DELETE como exemplos de manutenção dos dados.
12. Finalizar dizendo que o banco atende aos requisitos do projeto.

## 15. Observação final

O projeto foi feito de forma simples e legível para facilitar a explicação em sala de aula. A prioridade foi cumprir os requisitos do trabalho sem usar comandos avançados desnecessários.
