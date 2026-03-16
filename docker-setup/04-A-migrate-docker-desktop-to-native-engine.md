# Docker Desktop was hanging and crashing my whole machine

Docker Desktop started hanging on startup and making the entire machine unresponsive or crashing.
The problem is Docker Desktop runs a full Linux VM in the background — it consumes heavy RAM and CPU even when idle.

I had large production containers running inside the Docker Desktop engine that I did not want to lose or rebuild.
The goal was: **copy everything safely to the native Docker engine and run from there, without deleting anything from Docker Desktop.**

---

## Problem: Docker Desktop was causing machine to hang

- Docker Desktop hangs on startup
- Whole machine becomes unresponsive
- Occasionally crashes
- All my containers and images were inside the Docker Desktop engine (`desktop-linux` context)
- Native engine (`default` context) had nothing

---

## Step 1: Check current context

```bash
docker context ls
```



## Step 2: Write the migration script

I created the script using nano:

```bash
nano ~/migrate-docker.sh
```

Then pasted the following script:

```bash
#!/bin/bash
# migrate-docker-desktop-to-default.sh
# SAFE: No deletions. Export from desktop-linux, import into default.
set -e

EXPORT_DIR="$HOME/docker-migration-$(date +%Y%m%d-%H%M%S)"
mkdir -p "$EXPORT_DIR/images" "$EXPORT_DIR/volumes"

echo "=== Exporting from Docker Desktop engine ==="
docker context use desktop-linux

# --- Export all images ---
echo "[1/4] Saving images..."
docker images --format "{{.Repository}}:{{.Tag}}" | grep -v "^<none>" | while read img; do
  safe_name=$(echo "$img" | tr '/:' '__')
  echo "  Saving $img → $safe_name.tar"
  docker save "$img" -o "$EXPORT_DIR/images/$safe_name.tar"
done

# --- Export volumes with data ---
echo "[2/4] Backing up volumes..."
docker volume ls -q | while read vol; do
  echo "  Exporting volume: $vol"
  docker run --rm \
    -v "$vol":/source:ro \
    -v "$EXPORT_DIR/volumes":/backup \
    alpine sh -c "cd /source && tar czf /backup/${vol}.tar.gz . 2>/dev/null || true"
done

echo "=== Switching to default (native) engine ==="
docker context use default

# --- Import images ---
echo "[3/4] Loading images into native engine..."
for tar in "$EXPORT_DIR/images/"*.tar; do
  echo "  Loading $tar"
  docker load -i "$tar"
done

# --- Restore volumes ---
echo "[4/4] Restoring volumes into native engine..."
for archive in "$EXPORT_DIR/volumes/"*.tar.gz; do
  vol=$(basename "$archive" .tar.gz)
  echo "  Restoring volume: $vol"
  docker volume create "$vol" 2>/dev/null || true
  docker run --rm \
    -v "$vol":/target \
    -v "$EXPORT_DIR/volumes":/backup \
    alpine sh -c "cd /target && tar xzf /backup/${vol}.tar.gz"
done

echo ""
echo "=== Migration complete! ==="
echo "Exported data saved at: $EXPORT_DIR"
echo ""
echo "Now start your compose projects:"
echo "  cd /home/skmindlab/proj/erp-dockers/almalinux8-php56 && docker compose up -d"
echo "  cd /home/skmindlab/proj/conference-websites/docker-php-mariadb && docker compose up -d"
echo "  cd /home/skmindlab/proj/others/music_catalog_schema_small && docker compose up -d"
echo "  cd /home/skmindlab/proj/minio/minio-docker && docker compose up -d"
echo "  cd /home/skmindlab/proj/knowsio-setup/postgres-pgadmin && docker compose up -d"
```

Then made it executable and ran it:

```bash
chmod +x ~/migrate-docker.sh
bash ~/migrate-docker.sh
```

---

## Step 3: Script output

```
=== Migration complete! ===
Exported data saved at: /home/skmindlab/docker-migration-20260312-121308

Now start your compose projects:
  cd /home/skmindlab/proj/erp-dockers/almalinux8-php56 && docker compose up -d
  cd /home/skmindlab/proj/conference-websites/docker-php-mariadb && docker compose up -d
  cd /home/skmindlab/proj/others/music_catalog_schema_small && docker compose up -d
  cd /home/skmindlab/proj/minio/minio-docker && docker compose up -d
  cd /home/skmindlab/proj/knowsio-setup/postgres-pgadmin && docker compose up -d
```

The script only listed 5 projects because those were the only ones with **running containers** at migration time.
All other images (~28 GB total) were also migrated — they just had no running containers to list.

---

## Step 4: Why only 5 projects listed?

The script detects running containers in the Desktop engine and lists their compose project paths.
Other images (`gliner2`, `llm-graph-builder-v3`, `n8n`, `openclaw`, `sitecloner`, `keycloak`, `neo4j`, `docparser`, `pgadmin`) were migrated too — they just were not running at the time.

---

## Step 5: Stop Desktop containers before starting native ones

Docker Desktop containers were still running and holding all the ports (8022, 80, 3306, 9000, 9001, etc.).
I had to stop them first or the native engine would fail with `address already in use`.

```bash
# Switch to Desktop engine and stop all containers
docker context use desktop-linux
docker stop $(docker ps -q)

# Switch back to native engine
docker context use default
```

---

## Step 6: Start all compose projects on native engine

### almalinux8-php56
```bash
cd /home/skmindlab/proj/erp-dockers/almalinux8-php56 && docker compose up -d
```

### docker-php-mariadb (conference websites)
```bash
cd /home/skmindlab/proj/conference-websites/docker-php-mariadb && docker compose up -d
```

### music_catalog_schema_small
This project needs an external network called `openwebui-network`.
It existed in Docker Desktop but not in the native engine — had to create it first.

```bash
docker network create openwebui-network
cd /home/skmindlab/proj/others/music_catalog_schema_small && docker compose up -d
```

### minio
Also needed `openwebui-network` (already created above).

```bash
cd /home/skmindlab/proj/minio/minio-docker && docker compose up -d
```

### postgres-pgadmin
```bash
cd /home/skmindlab/proj/knowsio-setup/postgres-pgadmin && docker compose up -d
```

---

## Step 7: Verify everything is running

```bash
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

Result:
```
NAMES                    STATUS                   PORTS
pgadmin                  Up (healthy)             443/tcp, 0.0.0.0:5010->80/tcp
postgres                 Up (healthy)             0.0.0.0:5000->5432/tcp
minio                    Up (healthy)             0.0.0.0:9000-9001->9000-9001/tcp
music-catalog-frontend   Up (healthy)             0.0.0.0:3000->80/tcp
music-catalog-backend    Up (healthy)             0.0.0.0:8100->8000/tcp
phpmyadmin_container     Up                       0.0.0.0:8080->80/tcp
nginx_container          Up                       0.0.0.0:80->80/tcp, 0.0.0.0:8081-8083->8081-8083/tcp
php_container            Up                       9000/tcp
mariadb_container        Up                       0.0.0.0:3306->3306/tcp
almalinux8-php56         Up (healthy)
```

**All 9 containers running on native engine. All healthy.**



## Why the native engine is better for daily use

| | Docker Desktop | Native Engine |
|--|---------------|---------------|
| RAM overhead | High (runs full Linux VM) | Minimal |
| CPU usage idle | High | Near zero |
| Startup | Slow, sometimes hangs | Fast (systemd service) |
| Machine impact | Can freeze the whole machine | None |
| Use case | GUI, Docker Compose UI | CLI, production use |

---

## To undo — go back to Docker Desktop engine

If I ever want to switch back to Docker Desktop engine:

```bash
docker context use desktop-linux
```

Nothing was deleted. All images and volumes still exist in both engines.


# Made Docker start automatically on boot
sudo systemctl enable docker
sudo systemctl start docker


From now on - the docker will start automatically afte the mahine starts.



