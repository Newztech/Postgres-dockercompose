# PostgreSQL Setup Steps

## 1. Docker Installation
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh --dry-run
```

## 2. Create Directory Structure
```bash
# Create all required directories
mkdir -p /root/dba/postgres/{data,logs}

# Create .pgpass file directory
mkdir -p /root/dba/postgres
```

## 3. Generate Password
```bash
# Create and set permissions for .pgpass
touch /root/dba/postgres/.pgpass
openssl rand -base64 24 | tr -d '/+=' > /root/dba/postgres/.pgpass
chmod 600 /root/dba/postgres/.pgpass
```

## 4. Environment File
```bash
# Create and configure .env file
touch /root/dba/postgres/.env
cat > /root/dba/postgres/.env << EOL
POSTGRES_PASSWORD=$(cat /root/dba/postgres/.pgpass)
POSTGRES_USER=root
POSTGRES_DB=postgres
EOL
chmod 600 /root/dba/postgres/.env
```

## 5. Start Service
```bash
# For Normal PostgreSQL
docker compose -f docker-compose-normal.yml up -d

# For Percona PostgreSQL
docker compose -f docker-compose-percona.yml up -d
```

## 6. Database Connection
```bash
PGPASSWORD=$(cat /root/dba/postgres/.pgpass) psql -h localhost -U root -d postgres
```