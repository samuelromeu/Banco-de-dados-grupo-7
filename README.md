# Banco-de-dados-grupo-7
Trabalho final da disciplina Banco de Dados   
Opção 1: E-commerce tradicional  
integrantes do grupo 7

-Bernardo Miloski Pereira De Queiroz    
-Deiby Wilhan dos Santos oliveira Teixeira   
-Gustavo Dantas Senna Pinho   
-Maycon Oliveira Corrêa Statzner   
-Samuel Romeu Forster Dornellas   

# 1.Diagrama ER (primeira versão) 

![fd821179-126e-4ca8-b8e4-ebbbcd9ca866](https://github.com/samuelromeu/Banco-de-dados-grupo-7/assets/162327648/209e3f59-10e5-48bf-881d-c24710aede37)

# 2.Diagrama ER (depois da normalização) 

![f3abaeb6-23f9-4887-af80-dacfd1818324](https://github.com/samuelromeu/Banco-de-dados-grupo-7/assets/162327648/dd38408b-d7e9-4d07-ae86-aff90b7287a9)

# 3. SQL de criação das tabelas 

CREATE TABLE funcionarios (  
id SERIAL PRIMARY KEY,   
nome VARCHAR(150) NOT NULL,   
cpf VARCHAR(11) NOT NULL UNIQUE,    
ativo BOOL DEFAULT 'true',   
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,   
atualizado_em TIMESTAMP    
)   

CREATE TABLE clientes (  
id SERIAL PRIMARY KEY,  
nome_completo VARCHAR(150) NOT NULL,  
nome_de_usuario VARCHAR(50) not NULL,  
email VARCHAR(150) NOT NULL UNIQUE,  
CPF VARCHAR(11) NOT NULL UNIQUE,  
dt_nascimento DATE NOT NULL,  
ativo BOOL DEFAULT 'true',  
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
atualizado_em TIMESTAMP  
)  

CREATE TABLE telefones(  
id SERIAL PRIMARY KEY,  
prefixo VARCHAR(3) NOT NULL,  
DDD VARCHAR(2) NOT NULL,  
numero_telefone VARCHAR(9) NOT NULL UNIQUE,   
ativo BOOL DEFAULT 'true',  
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,   
atualizado_em TIMESTAMP,   
id_cliente INT REFERENCES clientes(id)   
)   

CREATE TABLE enderecos (  
id SERIAL PRIMARY KEY,   
CEP VARCHAR(8) NOT NULL,   
UF  VARCHAR(2) NOT NULL,  
cidade VARCHAR(50),   
bairro VARCHAR(50) NOT NULL,   
rua VARCHAR(75) NOT NULL,   
numero_casa INT NOT NULL,   
complemento VARCHAR(50),   
ativo BOOL DEFAULT 'true',   
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
atualizado_em TIMESTAMP,   
id_cliente INT REFERENCES clientes(id)  
)   

CREATE TABLE pedidos (   
id SERIAL PRIMARY KEY,  
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,   
atualizado_em TIMESTAMP,   
id_cliente INT REFERENCES clientes(id)   
)   

CREATE TABLE produtos (   
id SERIAL PRIMARY KEY,   
nome VARCHAR(100) NOT NULL,    
data_fabricacao TIMESTAMP NOT NULL,    
descricao VARCHAR(100) NOT NULL,    
valor_unitario REAL NOT NULL,   
data_validade DATE NOT NULL,   
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
atualizado_em TIMESTAMP,   
id_funcionario INT REFERENCES funcionarios(id)   
)   

CREATE TABLE estoque (  
id SERIAL NOT NULL,   
quantidade_estoque INT NOT NULL,   
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,   
atualizado_em TIMESTAMP,  
id_produto INT REFERENCES produtos(id),   
id_funcionario INT REFERENCES funcionarios(id)   
)   
  
CREATE TABLE categoria(   
id SERIAL PRIMARY KEY,   
nome VARCHAR(50) NOT NULL,   
descricao VARCHAR(100) NOT NULL,   
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,   
atualizado_em TIMESTAMP,  
id_produto INT REFERENCES produtos(id)   
)  

CREATE TABLE pedido_item (   
id_produto INT REFERENCES produtos(id),   
id_pedido INT REFERENCES pedidos(id),   
PRIMARY KEY (id_produto,id_pedido),   
quantidade_produtos INT NOT NULL   
)   

CREATE TABLE nota_fiscal (   
id SERIAL PRIMARY KEY,   
criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,   
id_pedido INT REFERENCES pedidos(id)   
)   

# 4. SQL de inserção de dados nas tabelas (pelo menos 5 registros em cada uma) 

INSERT INTO funcionarios   
    (nome,cpf)   
VALUES    
    ('samuel','12345678911'),    
    ('Maycon','11223344556'),   
    ('Bernardo','12332112390'),   
    ('Deiby','90901257381'),   
    ('Gustavo','22338834519')   

INSERT INTO clientes  
    (nome_completo,nome_de_usuario,email,cpf,dt_nascimento)    
VALUES    
    ('Sofia Martins da Silva','SofiaMSilva','SofiaMSilva@gmail.com','11111111111','1990/05/03/'),    
    ('Lucas Pereira Santos','LucasPSantos','LucasPSantos@yahoo.com','22222222222','1985/02/08/'),    
    ('Isabela Costa Oliveira','IsaCosta','IsaCosta@outlook.net','33333333333','1987/06/05'),    
    ('Rafael Mendes Almeida','MENDES','RafaMAlmeida@gmail.org','44444444444','1993/11/20'),    
    ('Ana Ferreira Rodrigues','Aninha','AnaFRodrigues@senai.com','55555555555','1980/09/10')    

INSERT INTO telefones    
    (prefixo,ddd,numero_telefone,id_cliente)    
VALUES     
    ('55','21','57382104','1'),    
    ('55','24','94627513','2'),    
    ('55','51','18205634','3'),   
    ('55','71','73928510','4'),    
    ('55','85','62049837','5')   
  
INSERT INTO enderecos    
    (cep,uf,cidade,bairro,rua,numero_casa,id_cliente,complemento)    
VALUES    
    ('25645430','RJ','Petrópolis','Centro','Rua do Imperador','210','1','Apartamento do lado da padaria'),   
    ('20520040','RJ','Rio de Janeiro','Tijuca','Rua dos santos','405','2','Edifício Santo Antônio'),   
    ('93310000','RS','Novo Hamburgo','Hamburgo Velho','Rua Lima','21','3','A esquerda da farmácia'),   
    ('44470000','BA','Vera Cruz','Barra do Gil','Rua da Saudade','141','4','A quarta casa da servidão'),   
    ('63900000','CE','Quixadá','Alto São Francisco','Rua Teixeira de Freitas','6','5','Subindo o calçadão a casa verde')   

INSERT INTO pedidos   
    (id_cliente)   
VALUES  
    (1),  
    (2),  
    (3),  
    (4),  
    (5)  

INSERT INTO nota_fiscal  
    (id_pedido)  
VALUES  
    (1),  
    (2),  
    (3),  
    (4),  
    (5)  

INSERT INTO produtos  
    (nome, data_fabricacao, descricao, valor_unitario, id_funcionario, data_validade)  
VALUES  
    ('Trakinas', '22/10/2023', 'Biscoito de chocolate recheado artificialmente com sabor de morango. Fabricado por Nabisco', '3.00', '1', '30/08/2024'),  
    ('Paçoquita', '10/11/2023', 'Doce de amendoim compactado. Fabricado por Santa Helena', '0.70', '2', '24/05/2024'),  
    ('Trident', '21/06/2023', 'Chiclete mastigável sabor artificial de Menta. Fabricado por Perfetti Van Melle', '2.50', '3', '25/12/2024'),  
    ('Pringles', '12/08/2023', 'Batata chips sabor artifical de Churrasco. Fabricado por Kellogg', '12.00', '4', '02/04/2025'),   
    ('Negresco', '07/09/2023', 'Biscoito de chocolate recheado artificialmente com sabor de Baunilha. Fabricado por Nestlé', '03.00', '5', '30/04/2024')   
    
INSERT INTO pedido_item   
    (id_produto, id_pedido, quantidade_produtos)   
VALUES  
    ('1', '1', '4'),  
    ('2', '2', '20'),  
    ('3', '3', '3'),  
    ('4', '4', '1'),  
    ('5', '5', '2')  
 
INSERT INTO categoria  
    (nome, descricao, id_produto)  
VALUES   
    ('Alimentícia', 'Biscoito industrializado', '1'),  
    ('Alimentícia', 'Doce industrializado', '2'),  
    ('Alimentícia', 'Chiclete industrializado', '3'),  
    ('Alimentícia', 'Batata chips industrializada', '4'),   
    ('Alimentícia', 'Biscoito industrializado', '5')   

INSERT INTO estoque  
    (quantidade_estoque, id_produto, id_funcionario)   
VALUES    
    ('200', '1', '1'),  
    ('2500', '2', '2'),   
    ('500', '3', '3'),   
    ('150', '4', '4'),   
    ('250', '5', '5')    

# 5. Um comando SQL de atualização em algum registro em uma tabela 

Atualiza o CPF de um dos funcionário

UPDATE funcionarios     
SET CPF = 09876543210,   
atualizado_em = CURRENT_TIMESTAMP   
WHERE ID = 1     

# 6. Um comando SQL de exclusão de algum registro em uma tabela

Deleta o número de telefone cadastrado por um cliente 
  
DELETE FROM telefones   
	WHERE id ='1'   
 
# 7. 5 SQLs de consulta 
 
-A. Pelo menos 2 com algum tipo de junção    
------------------------------------   
Indentifica nome do produto e quantidade no estoque referente 

SELECT     
p.nome,    
e.quantidade_estoque     
FROM      
produtos p      
JOIN estoque e ON e.id_produto = p.id     

Identifica o cliente e a cidade onde ele mora   

SELECT    
	c.nome_completo AS "Nomes_clientes",   
	e.cidade AS "Cidades "    
FROM  
	enderecos e    
	JOIN clientes c ON c.id = e.id_cliente   

-B. Pelo menos 1 com usando count() e group by()   
------------------------------------  

Indentifica a quantidade de pedidos que cada cliente fez no e-commerce  

SELECT    
	c.nome_completo AS "Nomes_clientes",   
	COUNT (p.id) AS "Numero_pedidos"   
FROM   
	clientes c  
	JOIN pedidos p ON p.id_cliente = c.id  
GROUP BY  
	c.nome_completo  

Identifica o número de clientes por estado

SELECT     
	COUNT (c.nome_de_usuario) AS "Numero_de_clientes",    
	e.UF AS "Estado"    
FROM    
	clientes c    
	JOIN enderecos e ON c.id = e.id_cliente   
GROUP BY      
	e.UF    

-C. 1 SQL para construção de nota fiscal
------------------------------------  

SELECT      
	nf.id AS "Id_nota",       
       c.nome_completo AS "Nome_Cliente",  
       c.cpf AS "CPF",   
       p.nome AS "Nome_Produto",    
       pi.quantidade_produtos AS "Quantidade",   
       pi.quantidade_produtos * p.valor_unitario AS "Valor_total"    
FROM nota_fiscal nf    
JOIN pedidos pd ON nf.id_pedido = pd.id   
JOIN clientes c ON pd.id_cliente = c.id    
JOIN pedido_item pi ON pd.id = pi.id_pedido    
JOIN produtos p ON pi.id_produto = p.id     
 
