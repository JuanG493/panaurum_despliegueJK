FROM mysql:8.0

# Copiar el script SQL al contenedor
COPY database/init.sql /docker-entrypoint-initdb.d/

# Ajustar permisos
RUN chmod 644 /docker-entrypoint-initdb.d/init.sql

# Exponer el puerto de MySQL
EXPOSE 3306
