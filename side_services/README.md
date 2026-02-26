# Side Services – Infrastruttura

Questa cartella contiene la configurazione Docker Compose per i servizi di infrastruttura necessari all'esecuzione di Medaryon.

---

## Servizi inclusi

| Servizio | Immagine | Porta | Descrizione |
|---|---|---|---|
| `mysql` | `mysql:8.0` | `3306` | Database relazionale |
| `nats` | `nats:2.10` | `4222` | Message broker (trasporto Moleculer) |
| `traefik` | `traefik:v2.11` | `80` / `8080` | Reverse proxy e load balancer |

---

## Prerequisiti

- **Docker** e **Docker Compose** installati

---

## Configurazione

Le variabili d'ambiente sono definite nel file `.env` presente in questa cartella:

```env
# Database
MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=moleculer
MYSQL_USER=moleculer
MYSQL_PASSWORD=moleculer
MYSQL_PORT=3306

# NATS
NATS_PORT=4222
```

Modifica i valori in base al tuo ambiente prima di avviare i servizi.

---

## Avvio

```bash
cd side_services
docker-compose up -d
```

Per verificare che i container siano in esecuzione:

```bash
docker-compose ps
```

---

## Arresto

```bash
docker-compose down
```

Per rimuovere anche i volumi (dati del database):

```bash
docker-compose down -v
```

---

## Log

```bash
# Log di tutti i servizi
docker-compose logs -f

# Log di un singolo servizio
docker-compose logs -f mysql
docker-compose logs -f nats
```

---

## Dashboard Traefik

La dashboard di Traefik è disponibile su:

```
http://localhost:8080
```

---

## Note

- Il file `database/init.sql` viene eseguito automaticamente alla prima creazione del container MySQL per inizializzare lo schema del database.
- I dati di MySQL vengono persistiti nel volume Docker `mysql_data`.
- Tutti i servizi condividono la rete interna `backend-medaryon`.
