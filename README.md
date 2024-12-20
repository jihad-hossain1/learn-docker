To access your `MySQL` database using **phpMyAdmin** through Docker, you'll need to set up a **phpMyAdmin** container and link it to your running **MySQL** container. Here's how you can do that:

### Step-by-Step Guide

#### 1. **Create a `docker-compose.yml` file (Recommended)**

Using **Docker Compose** makes it easier to manage multiple containers, especially when you want to link **MySQL** and **phpMyAdmin** together. Below is an example `docker-compose.yml` file for setting up both **MySQL** and **phpMyAdmin**:

Create a `docker-compose.yml` file in your project directory with the following contents:

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: mysql-container
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: my_new_database
    ports:
      - "3306:3306"
    networks:
      - mysql-network
    volumes:
      - mysql-data:/var/lib/mysql
    restart: always

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    environment:
      PMA_HOST: mysql-container
      PMA_USER: root
      PMA_PASSWORD: rootpassword
    ports:
      - "8080:80"
    networks:
      - mysql-network
    restart: always

networks:
  mysql-network:
    driver: bridge

volumes:
  mysql-data:
    driver: local
```
- Run this file using the command:
```bash
docker-compose up -d
```

The `docker run` command you’ve provided will run a MySQL container with the following configuration:

```bash
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=rootpassword -e MYSQL_DATABASE=my_new_database -d -p 3306:3306 mysql:8.0
```

Here’s a breakdown of what each option does:

- `--name mysql-container`: This names the container `mysql-container`, which you can use to reference it in other commands.
- `-e MYSQL_ROOT_PASSWORD=rootpassword`: Sets the root password for MySQL to `rootpassword`. You can change this to any strong password you prefer.
- `-e MYSQL_DATABASE=my_new_database`: Creates a new database named `my_new_database` when the container is started.
- `-d`: Runs the container in detached mode (in the background).
- `-p 3306:3306`: Maps port `3306` on your local machine to port `3306` inside the container, allowing you to access MySQL from your local machine.
- `mysql:8.0`: Uses the official `mysql` Docker image, specifically version `8.0`.

### Step-by-step usage:

1. **Run the Command**:

   To start the MySQL container, simply run:

   ```bash
   docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=rootpassword -e MYSQL_DATABASE=my_new_database -d -p 3306:3306 mysql:8.0
   ```

2. **Check If the Container is Running**:

   After running the command, you can verify that the container is up and running by executing:

   ```bash
   docker ps
   ```

   This will list all running containers. You should see `mysql-container` in the list.

3. **Access MySQL in the Container**:

   To connect to the MySQL container's MySQL instance, run:

   ```bash
   docker exec -it mysql-container mysql -uroot -prootpassword
   ```

   This command allows you to access MySQL via the `mysql` command-line client, using the `root` user and the password you set (`rootpassword`).

4. **Check Databases**:

   Once you’re inside MySQL, you can check the list of databases (including `my_new_database`) by running:

   ```sql
   SHOW DATABASES;
   ```

   5. **Create Databases**:

   To create a new database, run:

   ```sql
   CREATE DATABASE new_db;
   ```


5. **Access MySQL from External Tools**:

   If you want to access the MySQL instance from an external application (e.g., MySQL Workbench, phpMyAdmin, Prisma, or a custom app), use the following connection details:

   - **Host**: `localhost` (or your Docker host's IP address if running on a remote machine)
   - **Port**: `3306`
   - **Username**: `root`
   - **Password**: `rootpassword`
   - **Database**: `my_new_database`

---

### Example: Connecting with Prisma ORM

If you want to connect to the MySQL instance using Prisma ORM, you’ll typically configure it in your `prisma/schema.prisma` file like this:

```prisma
datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}
```

Then, in your `.env` file, you can define the `DATABASE_URL`:

```env
DATABASE_URL="mysql://root:rootpassword@localhost:3306/new_db"
```

### Optional: Stopping and Removing the Container

To stop the container when you’re done, use:

```bash
docker stop mysql-container
```

If you want to remove the container entirely, use:

```bash
docker rm mysql-container
```

### Summary:

- You can run MySQL in Docker with the `docker run` command.
- After running the container, you can connect to MySQL via `docker exec`.
- You can configure external tools (like Prisma) to connect to the MySQL instance by using `localhost:3306`.

Let me know if you have any more questions or run into any issues!

### Alternative: Manually Running phpMyAdmin with `docker run`

If you prefer not to use `docker-compose` and want to manually run the **phpMyAdmin** container, you can do that by linking the **phpMyAdmin** container to your existing **MySQL** container.

1. **Start phpMyAdmin**:

   Run the following command to start **phpMyAdmin** linked to your existing **MySQL** container:

   ```bash
   docker run --name phpmyadmin --link mysql-container:db -d -p 8080:80 -e PMA_HOST=mysql-container -e PMA_USER=root -e PMA_PASSWORD=rootpassword phpmyadmin/phpmyadmin
   ```


2. **Access phpMyAdmin**:
   You can access **phpMyAdmin** by navigating to:

   ```
   http://localhost:8080
   ```

---
