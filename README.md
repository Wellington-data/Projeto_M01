# 🏠 Datahouse de Hospedagens – PostgreSQL

Projeto de Banco de Dados Relacional para análise de performance de hospedagens, desenvolvido em PostgreSQL,
com foco em portfólio profissional, estudo e avaliação técnica (Júnior / Estágio).

## 📌 Sobre o Projeto

Este projeto implementa uma estrutura de dados analítica (Datahouse), permitindo:

📋 Análise de imóveis (listings)  
📝 Análise de avaliações (reviews)  
📍 Segmentação por bairros (neighbourhoods)  
💰 Cálculo de receita estimada  
📊 Consultas analíticas orientadas a negócio  
🚨 Identificação de ineficiências comerciais  

✅ Todas as regras de relacionamento e integridade são garantidas no banco de dados.  

---

## 🧠 Regras de Negócio

1. Cada imóvel pertence a um único bairro.  
2. Um imóvel pode ter várias avaliações.  
3. Cada avaliação está vinculada a um único imóvel.  
4. A receita estimada é calculada com base na ocupação:  

> price * (365 - availability_365)

5. Alta disponibilidade indica baixa ocupação.  
6. Baixa receita estimada indica baixo desempenho comercial.  

---

## 📂 Estrutura do Repositório:

📦 datahouse-hospedagens  
├── 📁 sql  
│   ├── 01_criacao_banco.sql                   
│   ├── 02_modelagem_tabelas.sql                 
│   ├── 03_carga_dados.sql                
│   ├── 04_consultas.sql                  
│
├── 📁 docs  
│   ├── DER.png                            
│   ├── dicionario_dados.md                
│   └── regras_negocio.md                
│
├── README.md  
└── .gitignore  

---

## 🛠️ Tecnologias Utilizadas:

1. 🐘 PostgreSQL  
2. 🧠 SQL (DDL e DML)  
3. 🗂️ Modelagem Relacional  
4. 🔗 Chaves Primárias e Estrangeiras  

---

## 🗄️ Scripts SQL  

### 📄 sql/01_criacao_banco.sql
  
```sql
CREATE DATABASE datahouse_hospedagens;
📄 sql/02_modelagem_tabelas.sql
\c datahouse_hospedagens;

CREATE TABLE neighbourhoods (
    neighbourhood VARCHAR(100) PRIMARY KEY
);

CREATE TABLE listings (
    id INT PRIMARY KEY,
    host_id INT NOT NULL,
    neighbourhood VARCHAR(100),
    price NUMERIC(10,2),
    availability_365 INT,

    CONSTRAINT fk_neighbourhood
        FOREIGN KEY (neighbourhood)
        REFERENCES neighbourhoods(neighbourhood)
);

CREATE TABLE reviews (
    id SERIAL PRIMARY KEY,
    listing_id INT,
    date DATE,

    CONSTRAINT fk_listing
        FOREIGN KEY (listing_id)
        REFERENCES listings(id)
        ON DELETE CASCADE
);
📄 sql/03_carga_dados.sql

Importação de dados via CSV (pgAdmin ou COPY):

COPY neighbourhoods FROM 'caminho/neighbourhoods.csv' DELIMITER ',' CSV HEADER;
COPY listings FROM 'caminho/listings.csv' DELIMITER ',' CSV HEADER;
COPY reviews FROM 'caminho/reviews.csv' DELIMITER ',' CSV HEADER;
📄 sql/04_consultas.sql

Hosts com baixo desempenho:

SELECT 
    host_id,
    COUNT(*) AS total_listings,
    AVG(availability_365) AS media_disponibilidade,
    AVG(price * (365 - availability_365)) AS media_receita
FROM listings
GROUP BY host_id
HAVING 
    AVG(availability_365) > 200
    AND AVG(price * (365 - availability_365)) < 5000;

Bairros com baixa atratividade:

SELECT 
    neighbourhood,
    COUNT(*) AS total_listings,
    AVG(availability_365) AS media_disponibilidade,
    AVG(price * (365 - availability_365)) AS media_receita
FROM listings
GROUP BY neighbourhood
HAVING 
    AVG(availability_365) > 200
    AND AVG(price * (365 - availability_365)) < 5000;

Receita estimada por imóvel:

SELECT 
    id,
    price,
    availability_365,
    (price * (365 - availability_365)) AS receita_estimada
FROM listings;

Quantidade de reviews por imóvel:

SELECT 
    listing_id,
    COUNT(*) AS total_reviews
FROM reviews
GROUP BY listing_id
ORDER BY total_reviews DESC;
📄 docs/DER.png

Diagrama Entidade-Relacionamento contendo:

neighbourhoods 1:N listings
listings 1:N reviews

(adicione sua imagem aqui)

📄 docs/dicionario_dados.md

(adicione aqui o dicionário de dados das tabelas)

📝 README.md
🏠 Datahouse de Hospedagens – PostgreSQL

Projeto de banco de dados relacional com foco em análise de desempenho de imóveis e geração de insights de negócio.

📌 Funcionalidades
Análise de performance de hosts
Identificação de imóveis ociosos
Avaliação de bairros com baixa demanda
Cálculo de receita estimada
Consultas analíticas estratégicas
🛠️ Tecnologias
PostgreSQL
SQL
▶️ Como Executar

Execute os scripts na ordem:

01_criacao_banco.sql
02_modelagem_tabelas.sql
03_carga_dados.sql
04_consultas.sql

📊 Consultas Incluídas
Quais hosts têm baixo desempenho?
Quais bairros são menos atrativos?
Qual a receita estimada dos imóveis?
Quais imóveis têm mais avaliações?
📈 Possíveis Evoluções

📊 Criação de views analíticas
⚙️ Automação de ETL
🧠 Machine Learning (previsão de preços)
📉 Integração com BI

👤 Autor
Wellington dos Santos

Estudante e entusiasta em Banco de Dados e Análise de Dados


---

Se quiser, posso te ajudar no próximo nível:

🔥 Gerar o **DER em imagem pronto**  
🔥 Criar o **dicionário de dados automaticamente**  
🔥 Revisar seu SQL pra ficar nível empresa  
🔥 Montar esse projeto como **case para entrevista técnica**

Só falar 🚀
