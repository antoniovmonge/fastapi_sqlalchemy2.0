# FastAPI, SQLAlchemy 2.0, Pydantic-V2, Alembic, Postgres and Docker

```bash
pip install -r requirements.txt
```

```bash
docker run \
  --rm   \
  --name postgres \
  -p 5555:5432 \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=postgres \
  -d postgres:14
```

```bash
$ docker exec -it postgres psql -U postgres

psql (14.7 (Debian 14.7-1.pgdg110+1))
Type "help" for help.

postgres=#
```

after removing the  `migrations` folder and the `alembic.ini` file:

```bash
alembic init -t async migrations
```

Edit `migration/env.py`

```bash
alembic revision --autogenerate -m 'add name_of_the_table'
```

check the database

```bash
$ docker exec -it postgres psql -U postgres

psql (14.7 (Debian 14.7-1.pgdg110+1))
Type "help" for help.

postgres=# \d
              List of relations
 Schema |      Name       | Type  |  Owner
--------+-----------------+-------+----------
 public | alembic_version | table | postgres
 public | users           | table | postgres
(2 rows)

postgres=# \d users
                           Table "public.users"
   Column   |            Type             | Collation | Nullable | Default
------------+-----------------------------+-----------+----------+---------
 id         | character varying           |           | not null |
 full_name  | character varying           |           |          |
 created_at | timestamp without time zone |           |          |
Indexes:
    "users_pkey" PRIMARY KEY, btree (id)
    "ix_users_created_at" btree (created_at)

postgres=#
```

run the application

```bash
uvicorn main:app --reload
```

check it out on <http://127.0.0.1:8000/docs>
