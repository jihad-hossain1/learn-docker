# Use the official MySQL image from the Docker Hub
FROM mysql:8.0

# Set environment variables for the MySQL root password and database name
ENV MYSQL_ROOT_PASSWORD=rootpassword
ENV MYSQL_DATABASE=mydatabase

# Expose the port MySQL will run on
EXPOSE 3306

# Optionally, copy a custom initialization script to the container (if needed)
# COPY ./init.sql /docker-entrypoint-initdb.d/

# The default command to run the MySQL service
CMD ["mysqld"]
