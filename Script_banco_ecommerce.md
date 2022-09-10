```sql
CREATE DATABASE if not exists ecommerce;
use ecommerce;

-- CRIAR TABELA CLIENTE
CREATE TABLE clients(
	idClient int auto_increment primary key,
    Fname VARCHAR(10),
    Minit CHAR(3),
    Lname VARCHAR(20),
    CPF CHAR(11) NOT NULL,
    Address VARCHAR(255),
    CONSTRAINT unique_cfp_client unique(CPF)
);

alter table clients auto_increment=1;

-- CRIAR TABELA PRODUTO
-- SIZE = DIMENSAO DO PRODUTO

CREATE TABLE product(
	idProduto int auto_increment primary key,
    Pname VARCHAR(30) NOT NULL,
    Classification_kids bool default false,
    category enum('Eletrônico', 'Vestimenta', 'Brinquedos', 'Alimentos', 'Móveis') not null,
    Avaliação float default 0,
    Size VARCHAR(10)
);

-- CRIAR TABELA PAYMENT

CREATE TABLE payments(
	idclient int,
    id_payment int,
    typePayment enum('Boleto', 'Cartão', 'Dois cartões'),
    limitAvailable float,
    primary key(idClient, id_payment)
);


-- CRIAR TABELA PEDIDO

CREATE TABLE orders(
	idOrders int auto_increment primary key,
    idOrders_Client int,
    orders_status enum('Cancelado', 'Confirmado', 'Em Processamento') default 'Em processamento',
    orders_description VARCHAR(255),
    send_Value float default 10,
    paymentCash bool default false,
    constraint fk_orders_client foreign key(idOrders_Client) references clients(idClient)
			on update cascade
);

-- CRIAR TABELA ESTOQUE

CREATE TABLE ProductStorage(
	idProdStorage int auto_increment primary key,
    storageLocation VARCHAR(255),
    quantity int default 0
);
-- drop table Seller;
CREATE TABLE Suppliers(
	idSupplier int auto_increment primary key,
    SocialName VARCHAR(255) NOT NULL,
    CNPJ CHAR(15),
    contact CHAR(11) NOT NULL,
    constraint unique_cnpj_seller unique(CNPJ)
);


-- CRIAR TABELA VENDEDOR

CREATE TABLE Seller(
	idSeller int auto_increment primary key,
    SocialName VARCHAR(255) NOT NULL,
    AbstName VARCHAR(255),
    CNPJ CHAR(15),
    CPF CHAR(9),
    location VARCHAR(255),
    contact CHAR(11) NOT NULL,
    constraint unique_cnpj_seller unique(CNPJ),
    constraint unique_cpf_seller unique(CPF)
);


CREATE TABLE productSeller(
	idPseller int,
    idProduct int,
    prodQuantity int default 1,
    primary key (idPseller, idProduct),
    constraint fk_product_seller foreign key (idPseller) references seller(idSeller),
    constraint fk_product_product foreign key (idProduct) references product(idProduto)
);


CREATE TABLE productOrder(
	idPOproduct int,
    idPOorder int,
    poQuantity int default 1,
    poStatus enum('Disponível', 'Sem Estoque') default 'Disponível',
    primary key (idPOproduct, idPOorder),
    constraint fk_productorder_seller foreign key (idPOproduct) references product(idProduto),
    constraint fk_productorder_product foreign key (idPOorder) references orders(idOrders)
);


CREATE TABLE StorageLocation(
	idLproduct int,
    idLstorage int,
    location VARCHAR(255) NOT NULL,
    primary key (idLproduct, idLstorage),
    constraint fk_storage_location_product foreign key (idLproduct) references product(idProduto),
    constraint fk_storage_location_storage foreign key (idLstorage) references ProductStorage(idProdStorage)
);

CREATE TABLE ProductSupplier(
	idPsSupplier int,
    idPsProduct int,
    quantity int not null,
    primary key (idPsSupplier, idPsProduct),
    constraint fk_product_supplier_supplier foreign key (idPsSupplier) references suppliers(idSupplier),
    constraint fk_product_supplier_product foreign key (idPsProduct) references product(idProduto)
);
```

--- ADICIONANDO VALORES AO BANCO --- 

``` sql
use ecommerce;

show tables;

insert into clients (Fname, Minit, Lname, CPF, Address)
		values('Maria', 'V', 'Silva', '123456789', 'Rua um 29, Centro - Cidade das Flores'),
			('Matheus', 'O', 'Pimentel', '987321654', 'Rua dois 64, Carangola - Cidade das Flores'),
            ('Ricardo', 'C', 'Silva', '458769321', 'Rua tres 451, Centro - Cidade das Flores'),
            ('Julia', 'G', 'França', '125463987', 'Rua quatro 2985, Centro - Cidade das Flores'),
            ('Roberta', 'F', 'Assis', '654216387', 'Rua cinco 274, Centro - Cidade das Flores'),
            ('Isabela', 'V', 'Cruz', '987456321', 'Rua seis 548, Centro - Cidade das Flores');
			
insert into product(Pname, Classification_kids, category, Avaliação, Size) values
							('Fone de ouvido', false, 'Eletrônico', '4', null),
                            ('Barbie Elsa', true, 'Brinquedos', '3', null),
                            ('Body Cartes', true, 'Vestimenta', '5', null),
                            ('Microfone Vedo - Youtuber', False, 'Eletrônico', '4', null),
                            ('Sofá retrátil', False, 'Móveis', '3', '3x57x80'),
                            ('Farinha de arroz', False, 'Alimentos', '2', null),
                            ('Fire Stick Amazon', False, 'Eletrônico', '3', null);
                            

-- select * from clients
-- select * from product


insert into orders (idOrders_Client, orders_status, orders_description, send_Value, paymentCash) values
						(1, default, 'compra via app', null, 1),
                        (2, default, 'compra via app', 50, 0),
                        (3, 'Confirmado', null, null, 1),
                        (4, default, 'compra via web site', 150,0);
                        
delete from orders where idOrders_Client in (1,2,3,4);

select * from orders;

insert into productOrder (idPOproduct, idPOorder, poQuantity, poStatus) values
							(1,5,2,null),
                            (2,5,1,null),
                            (3,6,1,null);
                            
                            
insert into ProductStorage (storageLocation, quantity) values
							('Rio de Janeiro', '1000'),
                            ('Rio de Janeiro', '500'),
                            ('São Paulo', '10'),
                            ('São Paulo', '100'),
                            ('São Paulo', '10'),
							('Brasília', '60');
                            
insert into StorageLocation (idLproduct, idLstorage, location) values
							(1,2,'RJ'),
                            (2,6, 'GO');
                            
                            
insert into Suppliers(SocialName, CNPJ, contact) values
						('Almeida e filhos', 123456789123456, '21985474'),
                        ('Eletrônicos Silva', 854519649143457, '21985484'),
                        ('Eletrônicos Valma', 934567893934695, '21975474');
                        
-- select * from Suppliers
                            
insert into ProductSupplier(idPsSupplier, idPsProduct, quantity) values
							(1,1,500),
                            (1,2,400),
                            (2,4,633),
                            (3,3,5),
                            (2,5,10); 
                            
insert into Seller( SocialName, AbstName, CNPJ, CPF, location, contact) values
					('Tech eletronics', null, 123456789456321, null, 'Rio de Janeiro', 219946287),
                    ('Botique Durgas', null, null, 123456783, 'Rio de Janeiro', 219567895),
                    ('Kids World', null, 456789123654485, null, 'São Paulo', 1198657484); 
                    
                    
insert into productSeller( idPseller, idProduct, prodQuantity) values
							(1,6,80),
              (2,7,10);
```


--- REALIZANDO QUERYS ---

``` sql

select * from  productSeller                         
                            
select count(*) from clients

select * from clients c, orders o where c.idClient = idOrders_Client;

select  from clients c, orders o where c.idClient = idOrders_Client;

select concat(Fname, ' ', Lname) as Client, idOrders as Request, orders_status as Stats from clients c, orders o where c.idClient = idOrders_Client;
                            
 select * from clients c inner join orders o ON c.idClient = o.idOrders_Client
