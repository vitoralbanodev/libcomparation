## 1. pyodbc

### Qual é o objetivo principal da biblioteca?
O `pyodbc` é uma biblioteca Python que implementa a especificação **DB API 2.0** e permite a comunicação com bancos de dados através do padrão **ODBC**. O ODBC é um padrão criado pela Microsoft que funciona como uma camada intermediária universal entre aplicações e diferentes sistemas de banco de dados, desde que exista um "driver ODBC" instalado para o banco em questão.

### Que tipo de banco de dados ela permite acessar?
Como trabalha por meio de drivers ODBC, o `pyodbc` é bastante flexível e pode acessar praticamente qualquer banco de dados que possua um driver ODBC disponível, como:
- Microsoft SQL Server
- MySQL
- PostgreSQL
- Oracle

### Ela é mais indicada para bancos relacionais ou não relacionais?
É indicada para **bancos de dados relacionais**. O ODBC foi projetado para bancos SQL, que trabalham com tabelas, linhas e colunas.

### A biblioteca trabalha com SQL puro, ORM ou ambos?
O `pyodbc` trabalha apenas com **SQL puro**. As consultas são escritas diretamente como strings SQL e enviadas ao banco através de um cursor.

### Como é feita a instalação?
A instalação é feita via `pip`:

```bash
pip install pyodbc
```

É necessário ter o **driver ODBC** do banco de dados instalado no sistema operacional (Ex: "ODBC Driver 17/18 for SQL Server").

### Como é criado um exemplo simples de conexão?

```python
import pyodbc

conexao = pyodbc.connect(
    'DRIVER={ODBC Driver 17 for SQL Server};'
    'SERVER=localhost;'
    'DATABASE=minha_base;'
    'UID=usuario;'
    'PWD=senha'
)

cursor = conexao.cursor()
print("Conexão realizada com sucesso!")
```

### Como executar uma consulta SELECT simples?

```python
cursor.execute("SELECT id, nome, email FROM clientes")

for linha in cursor.fetchall():
    print(linha)

conexao.close()
```

---

## 2. psycopg2

### Qual é o objetivo principal da biblioteca?
O `psycopg2` é o adaptador Python mais popular e completo para o **PostgreSQL**. Assim como o `pyodbc`, ele segue a especificação **DB API 2.0**, mas é uma biblioteca criada especificamente para se comunicar com o protocolo do PostgreSQL, sem depender de uma camada intermediária como o ODBC.

### Que tipo de banco de dados ela permite acessar?
Exclusivamente bancos de dados **PostgreSQL**.

### Ela é mais indicada para bancos relacionais ou não relacionais?
É indicada para **bancos de dados relacionais**, já que o PostgreSQL é um SGBD relacional.

### A biblioteca trabalha com SQL puro, ORM ou ambos?
Trabalha com **SQL puro**. Não é um ORM, mas é utilizada como "driver" por trás de ORMs populares, como o **SQLAlchemy** e o **Django ORM**, quando o banco utilizado é o PostgreSQL.

### Como é feita a instalação?
A instalação também é feita via `pip`. Em ambientes de desenvolvimento, geralmente se usa a versão binária, que é mais simples, pois já inclui as dependências compiladas:

```bash
pip install psycopg2-binary
```

Em ambientes de produção, recomenda-se compilar a partir do pacote `psycopg2` (sem o sufixo `-binary`), o que exige as bibliotecas de desenvolvimento do PostgreSQL instaladas no sistema.

### Como é criado um exemplo simples de conexão?

```python
import psycopg2

conexao = psycopg2.connect(
    host="localhost",
    dbname="minha_base",
    user="usuario",
    password="senha",
    port="5432"
)

cursor = conexao.cursor()
print("Conexão realizada com sucesso!")
```

### Como executar uma consulta SELECT simples?

```python
cursor.execute("SELECT id, nome, email FROM clientes")

for linha in cursor.fetchall():
    print(linha)

cursor.close()
conexao.close()
```

---

## 3. sqlite3

### Qual é o objetivo principal da biblioteca?
O `sqlite3` é um módulo que já vem **incluído na biblioteca padrão do Python**. Seu objetivo é permitir a conexão com bancos de dados **SQLite**, um SGBD leve, embutido e que armazena todo o banco em um único arquivo local, sem necessidade de um servidor de banco de dados separado.

### Que tipo de banco de dados ela permite acessar?
Apenas bancos de dados **SQLite**.

### Ela é mais indicada para bancos relacionais ou não relacionais?
É indicada para **bancos de dados relacionais**. O SQLite implementa boa parte do padrão SQL e trabalha com tabelas relacionadas por chaves primárias e estrangeiras.

### A biblioteca trabalha com SQL puro, ORM ou ambos?
Trabalha apenas com **SQL puro**.

### Como é feita a instalação?
Não é necessária instalação, pois o `sqlite3` já faz parte da biblioteca padrão do Python. Basta importar o módulo:

```python
import sqlite3
```

### Como é criado um exemplo simples de conexão?

```python
import sqlite3

conexao = sqlite3.connect("minha_base.db")
cursor = conexao.cursor()
print("Conexão realizada com sucesso!")
```

### Como executar uma consulta SELECT simples?

```python
cursor.execute("SELECT id, nome, email FROM clientes")

for linha in cursor.fetchall():
    print(linha)

conexao.close()
```
