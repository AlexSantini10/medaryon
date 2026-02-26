# Medaryon

Medaryon è una piattaforma a **microservizi** per il dominio medicale, costruita con **Moleculer.js**.  
Raccoglie in un unico ecosistema la gestione utenti, prenotazione appuntamenti, disponibilità dei dottori, referti medici, notifiche e monitoraggio delle attività.

---

## Panoramica del repository

```
medaryon/               → codice sorgente dell'applicazione (Node.js / Moleculer)
side_services/          → infrastruttura di supporto (MySQL, NATS, Traefik via Docker Compose)
python_test_e2e/        → test end-to-end e stress test in Python
```

---

## Documentazione per sotto-cartella

| Cartella | Descrizione | README |
|---|---|---|
| [`medaryon/`](./medaryon/README.md) | Codice sorgente, architettura e avvio dell'applicazione | [📖 Leggi](./medaryon/README.md) |
| [`side_services/`](./side_services/README.md) | Infrastruttura (database, message broker, reverse proxy) | [📖 Leggi](./side_services/README.md) |
| [`python_test_e2e/`](./python_test_e2e/README.md) | Test E2E e stress test | [📖 Leggi](./python_test_e2e/README.md) |

---

## Avvio rapido

1. Avvia l'infrastruttura → [`side_services/`](./side_services/README.md)
2. Avvia l'applicazione → [`medaryon/`](./medaryon/README.md)
3. Esegui i test → [`python_test_e2e/`](./python_test_e2e/README.md)
