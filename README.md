# What is this repo for?
We have postgres database and we aim to have the sqlalchemy model class auto-generated and ready to use.

# How to get there?

## prepare demo postgres db
```bash
: install docker-compose ref. bit.ly/nndocker

# create psql instance at port 54322         
: git clone folder https://github.com/namgivu/docker-compose-services
cd /path/to/git-cloned/docker-compose-services/
cd postgres/
./up.sh
docker ps | grep postgres
you_should_see='
229dca3ca7b5    mdillon/postgis    "docker-entrypoint.s…"    3 months ago    Up 7 hours    0.0.0.0:54322->5432/tcp    nn_postgres
'

# create db & table :customers
docker exec -it nn_postgres psql -Upostgres -c 'DROP DATABASE IF EXISTS nn_sqlacodegen;' 
docker exec -it nn_postgres psql -Upostgres -c 'CREATE DATABASE nn_sqlacodegen;' 
docker exec -it nn_postgres psql -Upostgres nn_sqlacodegen -c '
    DROP TABLE IF EXISTS customers;
    CREATE TABLE customers(
        id   SERIAL primary key,
        name TEXT, 
        dob  DATE, 
        updated_at TIMESTAMP
    );
'

```

## install sqlacodegen
ref. https://pypi.org/project/sqlacodegen/

```bash
: install python 3.6.x via pyenv ref. bit.ly/nnpipenv
: install pipenv ref. bit.ly/nnpipenv
pyenv local 3.6.9
pipenv install sqlacodegen
```


## generate sqlalchemy model class
ref. https://pypi.org/project/sqlacodegen/#example-usage

```bash
sqlacodegen postgresql://postgres:@localhost:54322/nn_sqlacodegen | tee customer.g.py  # .g. aka .generated.
```
