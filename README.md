### Step #21

## MySQL Setup

### Start boot2docker
```
boot2docker start
```

### Export environment variables
```
export DOCKER_HOST=tcp://192.168.59.103:2376
export DOCKER_CERT_PATH=/Users/jorge/.boot2docker/certs/boot2docker-vm
export DOCKER_TLS_VERIFY=1
```

### Start the main db:
```
docker run --name db -e MYSQL_ROOT_PASSWORD=test -d -p 3306:3306 mariadb
```

### Start a client:
```
docker run --name mysql-client -it --link db:mysql --rm mariadb sh -c 'exec mysql -uroot -ptest -hmysql'
```

### Create a database

Show databases
```
SHOW DATABASES
```

Create the database
```
CREATE DATABASE my_flask_app;
```

Check the database exists
```
SHOW DATABASES
```

Select the database
```
USE my_flask_app;
```

Create the User table
```
CREATE TABLE user(
user_id INT NOT NULL AUTO_INCREMENT,
username VARCHAR(64) NOT NULL,
password VARCHAR(40) NOT NULL,
PRIMARY KEY(user_id)
);
```

Check the table exists
```
SHOW TABLES;
```

Insert a record to make sure the table works
```
insert into user values('','jorge','12345');
```

Check the record exists
```
SELECT * FROM user;
```

## Tie the web app with the db app

Regenerate the container with the new requirements.txt (flask-mysql)

```
docker build -t flask-intro-mysql .
```

Run the web container as:
```
docker run -d -p 5000:5000 -v /Users/jorge/flask-intro:/opt/flask-intro --name web --link db:mysql flask-intro-mysql
```
