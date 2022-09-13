```sql
CREATE DATABASE if not exists banco_oficina;

use banco_oficina


-- CRIANDO TABELA CLIENTE

CREATE TABLE clients(
	idClient int unsigned auto_increment primary key,
    CPF char(11),
    NOME varchar(100),
    CONTATO char(11),
    constraint unique_cpf unique(CPF)
);

alter table clients auto_increment=1;

-- CRIANDO TABELA OrdemDeServico

CREATE TABLE if not exists Ordem_De_Servico(
	idOrderDeServico int unsigned auto_increment primary key,
    OS_idCliente int unsigned NOT NULL,
    Data_Emissao DATE,
    Data_Conclusao DATE,
    Valor DECIMAL(10,2),
    OsStatus bool,
    constraint fk_OrdemDeServico foreign key (OS_idCliente) references clients(idClient)
);


-- CRIANDO TABELA Equipe_mecanicos

CREATE TABLE if not exists Equipe_Mecanicos(
	idEquipe_Mecanicos int unsigned auto_increment primary key,
    Responsável VARCHAR(45),
    Data_Entrega DATE,
    Serviço_realizado enum('Concerto', 'Revisão') not null,
    EquipeMecanicos_idOS int unsigned not null
);


-- CRIANDO TABELA VEÍCULO

CREATE TABLE if not exists Veiculo(
	idVeiculo int unsigned auto_increment primary key,
    Modelo VARCHAR(45),
    Ano CHAR(4),
    Placa CHAR(7),
    Municipio VARCHAR(50),
    Veiculo_idCliente int unsigned not null,
    Veiculo_iDequipe_mecanicos int unsigned not null,
	constraint fk_veiculo0 foreign key (Veiculo_idCliente) references clients(idClient),
    constraint fk_veiculo1 foreign key (Veiculo_iDequipe_mecanicos) references Equipe_Mecanicos(idEquipe_Mecanicos)
);


-- CRIANDO TABELA VALOR DE REFERÊNCIA

CREATE TABLE if not exists Valor_Referencia(
	idValorReferencia int unsigned auto_increment primary key,
    Valor DECIMAL(10,2),
    VR_idOrderDeServico int unsigned not null,
    constraint fk_valorReferencia foreign key (VR_idOrderDeServico) references Ordem_De_Servico (idOrderDeServico)
);

-- CRIANDO TABELA DE PEÇAS

CREATE TABLE if not exists Pecas(
	idPeca int unsigned auto_increment primary key,
    Nome VARCHAR(45),
    Valor DECIMAL(10,2),
    Peca_idOrderDeServico int unsigned not null,
    constraint fk_pecas foreign key(Peca_idOrderDeServico) references Ordem_De_Servico(idOrderDeServico)
);

-- CRIANDO TABELA MECANICOS

CREATE TABLE if not exists Mecanicos(
	idMecanicos int unsigned auto_increment primary key,
    Nome VARCHAR(45),
    Endereço VARCHAR(255),
    Especialidade VARCHAR(100),
    Equipe_idEquipe_Mecanicos int unsigned,
    constraint fk_mecanico foreign key(Equipe_idEquipe_Mecanicos) references Equipe_Mecanicos(idEquipe_Mecanicos)
);


-- INSERINDO DADOS NO BANCO DE DADOS

INSERT INTO clients(CPF, NOME, CONTATO) values
	('12345678905', 'Aluisio Souza', '17145236525'),
    ('74586932541', 'Emilia Costa', '41598463214'),
    ('74521365236', 'Fernanda Perez', '47856321548'),
    ('95648721365', 'Diego Partes', '95642187649'),
    ('24513658754', 'Erick Silva', '32654157894')
;

INSERT INTO  Ordem_De_Servico(OS_idCliente, Data_Emissao, Data_Conclusao, Valor, OsStatus) values
	(1, STR_TO_DATE( "03/10/2016", "%m/%d/%Y" ), STR_TO_DATE( "04/27/2016", "%m/%d/%Y" ), 250, true),
    (2, STR_TO_DATE( "03/10/2016", "%m/%d/%Y" ), STR_TO_DATE( "04/27/2016", "%m/%d/%Y" ), 375, true),
    (3, STR_TO_DATE( "03/10/2016", "%m/%d/%Y" ), STR_TO_DATE( "04/27/2016", "%m/%d/%Y" ), 120, false),
    (4, STR_TO_DATE( "03/10/2016", "%m/%d/%Y" ), STR_TO_DATE( "04/27/2016", "%m/%d/%Y" ), 137, false),
    (5, STR_TO_DATE( "03/10/2016", "%m/%d/%Y" ), STR_TO_DATE( "04/27/2016", "%m/%d/%Y" ), 346, true)
;

 INSERT INTO Veiculo(Modelo, Ano, Placa, Municipio, Veiculo_idCliente, Veiculo_iDequipe_mecanicos) values
	('Civic', '2014', 'ABC2497', 'Barra Bonita', 1,1),
    ('Opala', '2003', 'BDG4578', 'Barra Bonita', 2,2),
    ('Frontier', '2013', 'GHF8745', 'Barra Bonita', 3,3),
    ('Prisma', '2022', 'KLJ1234', 'Barra Bonita', 4,4),
    ('Cobalt', '2021', 'AGF6549', 'Barra Bonita', 5,5)
;


INSERT INTO  Valor_Referencia(Valor, VR_idOrderDeServico) values 
	(128.00, 1),(97.00, 2);


INSERT INTO Pecas(Nome, Valor, Peca_idOrderDeServico) values 
	('Óleo', 80.00, 3),
    ('Pastilha de freio', 240.00, 2),
    ('Parabrisas', 350.00, 1),
    ('Rodas', 1.500, 5),
    ('Parachoque', 700.00, 4)
;


INSERT INTO Equipe_Mecanicos(Serviço_realizado, Data_Entrega, EquipeMecanicos_idOS) values 
	('Concerto', STR_TO_DATE( "04/27/2016", "%m/%d/%Y"), 1),
    ('Revisão', STR_TO_DATE( "04/27/2016", "%m/%d/%Y"), 2),
    ('Concerto', STR_TO_DATE( "04/27/2016", "%m/%d/%Y"), 3),
    ('Revisão', STR_TO_DATE( "04/27/2016", "%m/%d/%Y"), 4),
    ('Concerto', STR_TO_DATE( "04/27/2016", "%m/%d/%Y"), 5)
;

INSERT INTO Mecanicos(Nome, Endereço, Especialidade, Equipe_idEquipe_Mecanicos) values
	('Cristovão Ezequias', 'Rua dos anjos', 'Mecânico', 1),
    ('Joao Alberto', 'Rua dos limoedos', 'Mecânico', 1),
    ('Jesus dos Anjos', 'Rua dos guarás', 'Mecânico', 1),
    ('Luciano Azevedo', 'Rua dos filhos', 'Mecânico', 1),
    ('Joao Alcides', 'Rua dos ticós', 'Mecânico', 1)
;


-- QUERYS -- 

SELECT * FROM clients
select * from Ordem_De_Servico o, Valor_Referencia v, Pecas p where v.idValorReferencia = o.idOrderDeServico and p.Peca_idOrderDeServico = o.idOrderDeServico;
```
