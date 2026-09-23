# Bibliotecas Python para Conexão com Bancos de Dados: pyodbc, psycopg2 e sqlite3

## Introdução

Em aplicações reais, o acesso a bancos de dados é uma das operações mais frequentes de um sistema. No ecossistema Python, existem diversas bibliotecas especializadas em conectar aplicações a diferentes tipos de banco de dados. Este documento pesquisa, compara e apresenta três dessas bibliotecas — **pyodbc**, **psycopg2** e **sqlite3** — respondendo a um conjunto de perguntas-guia sobre cada uma delas.

---

## 1. pyodbc

### Qual é o objetivo principal da biblioteca?
O `pyodbc` é uma biblioteca Python que implementa a especificação **DB API 2.0** e permite a comunicação com bancos de dados através do padrão **ODBC** (Open Database Connectivity). O ODBC é um padrão criado pela Microsoft que funciona como uma camada intermediária universal entre aplicações e diferentes sistemas de banco de dados, desde que exista um "driver ODBC" instalado para o banco em questão.

### Que tipo de banco de dados ela permite acessar?
Como trabalha por meio de drivers ODBC, o `pyodbc` é bastante flexível e pode acessar praticamente qualquer banco de dados que possua um driver ODBC disponível, como:
- Microsoft SQL Server
- Microsoft Access
- MySQL
- PostgreSQL
- Oracle
- IBM DB2

Por isso, é a biblioteca mais comum quando o projeto precisa se conectar a bancos da Microsoft, especialmente o **SQL Server**.

### Ela é mais indicada para bancos relacionais ou não relacionais?
É indicada para **bancos de dados relacionais**. O ODBC foi projetado para bancos SQL, que trabalham com tabelas, linhas e colunas.

### A biblioteca trabalha com SQL puro, ORM ou ambos?
O `pyodbc` trabalha apenas com **SQL puro**. As consultas são escritas diretamente como strings SQL e enviadas ao banco através de um cursor. Ele não é um ORM, mas pode ser usado como camada de conexão por baixo de ORMs (como o SQLAlchemy, quando configurado com um dialeto ODBC).

### Como é feita a instalação?
A instalação é feita via `pip`:

```bash
pip install pyodbc
```

É importante observar que, além da biblioteca Python, é necessário ter o **driver ODBC** do banco de dados instalado no sistema operacional (por exemplo, o "ODBC Driver 17/18 for SQL Server" para conectar ao SQL Server).

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
O `psycopg2` é o adaptador Python mais popular e completo para o **PostgreSQL**. Assim como o `pyodbc`, ele segue a especificação **DB API 2.0**, mas é uma biblioteca criada especificamente (nativamente) para se comunicar com o protocolo do PostgreSQL, sem depender de uma camada intermediária como o ODBC.

### Que tipo de banco de dados ela permite acessar?
Exclusivamente bancos de dados **PostgreSQL**.

### Ela é mais indicada para bancos relacionais ou não relacionais?
É indicada para **bancos de dados relacionais**, já que o PostgreSQL é um SGBD relacional (embora possua suporte a tipos JSON/JSONB, o `psycopg2` continua sendo uma ferramenta voltada ao modelo relacional).

### A biblioteca trabalha com SQL puro, ORM ou ambos?
Trabalha com **SQL puro**. Não é um ORM, mas é amplamente utilizada como "driver" por trás de ORMs populares, como o **SQLAlchemy** e o **Django ORM**, quando o banco utilizado é o PostgreSQL.

### Como é feita a instalação?
A instalação também é feita via `pip`. Em ambientes de desenvolvimento, geralmente se usa a versão binária (mais simples, pois já inclui as dependências compiladas):

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
O `sqlite3` é um módulo que já vem **incluído na biblioteca padrão do Python** (não precisa ser instalado separadamente). Seu objetivo é permitir a conexão com bancos de dados **SQLite**, um SGBD leve, embutido (embedded) e que armazena todo o banco em um único arquivo local, sem necessidade de um servidor de banco de dados separado.

### Que tipo de banco de dados ela permite acessar?
Apenas bancos de dados **SQLite**.

### Ela é mais indicada para bancos relacionais ou não relacionais?
É indicada para **bancos de dados relacionais**. O SQLite implementa boa parte do padrão SQL e trabalha com tabelas relacionadas por chaves primárias e estrangeiras.

### A biblioteca trabalha com SQL puro, ORM ou ambos?
Trabalha apenas com **SQL puro**. Assim como as anteriores, não é um ORM, mas serve de base para ORMs (o SQLAlchemy e o Django ORM, por exemplo, suportam SQLite nativamente usando esse módulo por baixo dos panos).

### Como é feita a instalação?
Não é necessária instalação, pois o `sqlite3` já faz parte da biblioteca padrão do Python (desde a versão 2.5). Basta importar o módulo:

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

Se o arquivo `minha_base.db` não existir, o próprio `sqlite3` cria um novo automaticamente.

### Como executar uma consulta SELECT simples?

```python
cursor.execute("SELECT id, nome, email FROM clientes")

for linha in cursor.fetchall():
    print(linha)

conexao.close()
```

---

## 4. Quadro comparativo

| Característica          | pyodbc                              | psycopg2                     | sqlite3                        |
|--------------------------|--------------------------------------|-------------------------------|----------------------------------|
| Banco(s) suportado(s)     | Vários, via driver ODBC (SQL Server, Access, MySQL, etc.) | PostgreSQL                    | SQLite                          |
| Tipo de banco             | Relacional                          | Relacional                    | Relacional                      |
| SQL puro ou ORM           | SQL puro                            | SQL puro                      | SQL puro                        |
| Necessita instalação extra| Sim (biblioteca + driver ODBC do SO) | Sim (via pip)                 | Não (já vem no Python padrão)   |
| Necessita servidor externo| Sim (na maioria dos casos)          | Sim (servidor PostgreSQL)     | Não (arquivo local)             |
| Cenário de uso típico     | Integração com bancos Microsoft / ambientes corporativos heterogêneos | Aplicações que usam PostgreSQL como SGBD principal | Protótipos, aplicações pequenas, testes, aplicativos desktop/mobile |

---

## 5. Conclusão

As três bibliotecas cumprem o mesmo papel fundamental — permitir que uma aplicação Python execute comandos SQL em um banco de dados relacional — mas se diferenciam pelo escopo de bancos que suportam e pela forma de conexão:

- **pyodbc** é a escolha mais flexível quando o projeto precisa se conectar a diferentes SGBDs por meio do padrão ODBC, sendo muito usada com **SQL Server**.
- **psycopg2** é a biblioteca de referência para projetos que utilizam especificamente o **PostgreSQL**, oferecendo melhor desempenho e integração nativa com esse banco.
- **sqlite3** é ideal para cenários simples, protótipos, testes automatizados e aplicações que não precisam de um servidor de banco de dados dedicado, já que todo o banco fica em um único arquivo local.

Nenhuma delas é um ORM — todas trabalham com SQL puro — mas todas podem servir de base ("driver") para ORMs como SQLAlchemy e Django ORM.
