# DataTalksClub Data Engineering
In this repository, I create a data pipeline using data from New York City's Taxi and Limousing Commission following the DataTalksClub Data Engineering course.

In this README, I will log my progress.

## Docker Intro

To run a container with python 3.9 and then use the bash.
This way you can install other packages like pandas.
```
$ docker run -t --entrypoint=bash python:3.9
```
```
$ pip install pandas
```
However, when you open the image again, pandas will not be there.

To make sure it is, you can specify it in a docker file.

Write:
```
FROM python:3.9

RUN pip install pandas

ENTRYPOINT ["bash"]
```

Then you run:
```
$ docker build -t test:pandas .
```

This installs python and pandas.

Then run:

```
$ docker run -it test:pandas
```


### Docker and Postgres

Run docker with the following script

```
$ docker run -it \
 -e POSTGRES_USER="root" \
 -e POSTGRES_PASSWORD="root" \
 -e POSTGRES_DB="ny_taxi" \
 -v PATH/ny_taxi_postgres_data:/var/lib/postgresql/data \
 -p 5432:5432 \
 postgres:13
```

Then run a cli client to access the database

You can use the cli to run queries on the db

NOTE: cli did not work for me, so I used pgAdmin instead

NOTE: This helped me fix the issue with pgcli:
https://github.com/sameersbn/docker-postgresql/issues/112
and in particular this: https://narayanatutorial.com/database/postgresql/how-to-start-and-stop-postgresql-database-in-windows


It is preferable to use conda in the terminal and run:

```
$ pgcli -h localhost -p 5432 -u root
```

Now load the dataset into postgres docker

Some ways to check a csv file in the terminal:
```
$ less __.csv
```
displays some rows
```
$ head -n 100 __.csv > yellow_head.csv
```
takes the first 100 lines and copies them to a file
```
$ wc -l __.csv
```
counts the number of lines in the file

Work on jupyter python to upload data in chunks.

It throws an exception, which is not great, but the data loaded fine.

### PGADMIN

Now use pgadmin. To do this run a container with pgadmin. To connect to the other container, create a network.

Run the container:
```
$ docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v c:/Users/lfpon/source/repos/datatalks/2_docker_sql/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
```

Check if the database is still there:
```
$ pgcli -h localhost -p 5432 -u root -d ny_taxi
```
and run the query:
```
$ SELECT COUNT(1) FROM yellow_taxi_data
```
Then exit pgcli and run the docker container with pgadmin:
```
$ docker run -it \
    -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
    -e PGADMIN_DEFAULT_PASSWORD="root" \
    -p 8080:80 \
    --network=pg-network \
    --name pgadmin \
    dpage/pgadmin4
```

Next... Dockerizing the Ingestion Script


Data source:
www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page
