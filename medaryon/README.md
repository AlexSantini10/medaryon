# Medaryon – Applicazione

Medaryon è una piattaforma a microservizi per il dominio **medicale**, costruita con **Moleculer.js**.  
L'obiettivo è duplice:
1. Mostrare come Moleculer consenta di creare applicazioni distribuite, scalabili e modulari.
2. Produrre un esempio concreto e facilmente estendibile che integri più microservizi con logiche reali di coordinamento.

---

## Architettura

Ogni microservizio è indipendente ma interconnesso tramite il broker di Moleculer, che utilizza **NATS** come trasporto.  
Questo consente di avviarli separatamente, ridistribuirli su più nodi e bilanciare il carico automaticamente.

L'attivazione dei servizi è gestita tramite **dotenv**: la variabile `NODE_SERVICES` nel file `.env` definisce quali servizi avviare su un nodo specifico.

```
Client HTTP
    │
    ▼
[gateway]  (moleculer-web, porta 3000)
    │
    ▼ (NATS)
┌──────────────────────────────────────┐
│  users · appointments · availability │
│  reports · payments · logs           │
│  notifications · openapi             │
└──────────────────────────────────────┘
    │
    ▼
  MySQL
```

---

## Servizi implementati

| Servizio | Descrizione |
|---|---|
| `users` | Registrazione, login e gestione ruoli (patient / doctor) |
| `appointments` | Prenotazione, aggiornamento stato e riprogrammazione degli appuntamenti |
| `availability` | Disponibilità e fasce orarie dei dottori |
| `reports` | Caricamento e accesso ai referti medici |
| `payments` | Creazione e gestione dei pagamenti |
| `notifications` | Invio notifiche (email, push, logiche custom) |
| `logs` | Tracciamento delle operazioni (activity log) |
| `gateway` | API gateway che espone i servizi all'esterno via HTTP |
| `openapi` | Generazione automatica della documentazione OpenAPI |

---

## Struttura delle directory

```
medaryon/
├── services/
│   ├── appointments/
│   ├── availability/
│   ├── logs/
│   ├── notifications/
│   ├── openapi/
│   ├── payments/
│   ├── reports/
│   └── users/
├── gateway/              # configurazione dell'API gateway
├── mixins/               # mixin condivisi tra i servizi
├── errors/               # classi di errore personalizzate
├── public/               # file statici esposti dal gateway
├── test/                 # test unitari (Jest)
├── index.js              # entry point
├── moleculer.config.js   # configurazione del broker Moleculer
├── .env                  # variabili d'ambiente (non committare in produzione)
├── docker-compose.yml    # avvio tramite Docker
└── package.json
```

Ogni servizio segue questa struttura base:

```
services/nome-servizio/
├── nome-servizio.service.js   # definizione principale del servizio
├── actions/                   # azioni esposte dal servizio
├── events/                    # eventi ascoltati o emessi
├── models/                    # definizione dei modelli dati
└── utils/                     # funzioni di supporto
```

---

## Prerequisiti

- **Node.js** >= 16
- **MySQL** e **NATS** in esecuzione (vedi [`../side_services/`](../side_services/README.md))

---

## Configurazione

Copia il file `.env` e adatta le variabili all'ambiente:

```env
NODE_ID=node-users-1
NODE_SERVICES=users,appointments,availability,logs,payments,reports,notifications,openapi

TRANSPORTER=nats://127.0.0.1:4222

DB_HOST=localhost
DB_PORT=3306
DB_USER=moleculer
DB_PASSWORD=moleculer
DB_NAME=moleculer

JWT_SECRET=<secret-key>
ENV=development
```

---

## Avvio

### Avvio locale (Node.js)

```bash
# 1. Installa le dipendenze
npm install

# 2. Avvia l'applicazione
npm start
```

L'API gateway sarà disponibile su `http://localhost:3000`.

### Avvio con Docker Compose

```bash
docker-compose up --build -d
```

### Modalità sviluppo (hot-reload + REPL)

```bash
npm run dev
```

---

## Test unitari

```bash
npm test
```

Per la modalità watch durante lo sviluppo:

```bash
npm run ci
```

I report di copertura vengono generati nella cartella `../coverage/`.

---

## Linting

```bash
npm run lint
```

---

## Documentazione API

Una volta avviata l'applicazione, la documentazione OpenAPI è disponibile su:

```
http://localhost:3000/api/openapi.yaml
```
