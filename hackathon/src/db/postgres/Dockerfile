# TODO

# We use Postgres for the database, version 11.6 - it can use a minimal OS.

# There's an init script in this folder which populates the reference data:
#  init-products-db.sh

# Your base image should make it easy to run that script as part of the container startup.

#===================================================================
#=========================mikeprado=================================
# Use Alpine Linux as the base image
FROM alpine:3.18

# Set environment for non-interactive installs
ENV DEBIAN_FRONTEND=noninteractive
ENV POSTGRES_USER=mikeprado
ENV POSTGRES_PASSWORD=hackatonDbpw123
ENV POSTGRES_DB=postgres

# Install PostgreSQL and necessary utilities
RUN apk update && \
    apk add --no-cache \
    postgresql15 \
    postgresql-client \
    bash \
    curl \
    sudo && \
    rm -rf /var/cache/apk/*

# Create necessary directories and set permissions
RUN mkdir -p /var/lib/postgresql/data /docker-entrypoint-initdb.d/ /run/postgresql && \
    chown -R postgres:postgres /var/lib/postgresql /docker-entrypoint-initdb.d /run/postgresql && \
    chmod 775 /run/postgresql

# Copy the init script into the container
COPY init-products-db.sh /docker-entrypoint-initdb.d/

# Make sure the script is executable
RUN chmod +x /docker-entrypoint-initdb.d/init-products-db.sh && \
    chown postgres:postgres /docker-entrypoint-initdb.d/init-products-db.sh

# Expose PostgreSQL default port
EXPOSE 5432

# Switch to the postgres user for initialization
USER postgres

# Initialize PostgreSQL database
RUN initdb -D /var/lib/postgresql/data

# Start PostgreSQL server by default
CMD ["postgres", "-D", "/var/lib/postgresql/data"]