FROM postgres:16.6

# Install build dependencies for pgvector
RUN apt-get update && apt-get install -y \
    postgresql-server-dev-16 \
    make \
    gcc \
    git && \
    git clone https://github.com/pgvector/pgvector.git && \
    cd pgvector && \
    make && make install && \
    cd .. && rm -rf pgvector && \
    apt-get remove -y make gcc git && apt-get autoremove -y && apt-get clean

# Add pgvector and uuid-ossp extensions
RUN echo "CREATE EXTENSION IF NOT EXISTS vector;" >> /docker-entrypoint-initdb.d/init_extensions.sql && \
    echo "CREATE EXTENSION IF NOT EXISTS \"uuid-ossp\";" >> /docker-entrypoint-initdb.d/init_extensions.sql
