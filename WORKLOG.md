
## Basic 

```bash
docker-compose up -d db
docker-compose run --rm flaskapp /bin/bash -c "cd /opt/services/flaskapp/src && python -c  'import database; database.init_db()'"
docker-compose up -d
```

```bash
docker logs systems-puzzle_flaskapp_1
docker logs systems-puzzle_db_1
docker logs systems-puzzle_nginx_1 
```


## nginx

### Wrong port on flask app 

```bash
docker logs systems-puzzle_flaskapp_1
/usr/local/lib/python3.7/site-packages/psycopg2/__init__.py:144: UserWarning: The psycopg2 wheel package will be renamed from release 2.8; in order to keep installing from binary please use "pip install psycopg2-binary" instead. For details see: <http://initd.org/psycopg/docs/install.html#binary-install-from-pypi>.
  """)
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

Change port on nginx conf 
Change port Dockerfile 

Check network 


### Check location of nginx mapping for conf 

```bash
docker exec -it systems-puzzle_nginx_1 cat /etc/nginx/conf.d/flaskapp.conf
```

### Switched ports on nginx 

flaskapp.conf
```
proxy_pass http://flaskapp:5000;
```

## Flask app 

Probably good 

```bash
docker logs systems-puzzle_flaskapp_1
```

Needed to fix Dockerfile to right port

## Postgres 

Check environment variables -> ok

### Expose ports 5432 on docker compose 

```yaml
services:
  db:
    ports:
      - "5432:5432"
```

### Check networking 

```bash
docker exec -it systems-puzzle_db_1 /bin/bash
apt update 
apt install netcat -y
```

No need as the tables were populated at this point. 

### Verify 

```bash
psql -h localhost -U docker_pg flaskapp_db
>> select * from items;
```

Works!!

## Beyond
 
### Kubernetes 

- Convert compose to helm with kompose 
- Include CI to push images to dockerhub 
- Create user in container and run as user without root priv


