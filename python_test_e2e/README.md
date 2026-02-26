# Python Test E2E

Questa cartella contiene i test end-to-end e gli stress test per l'API di Medaryon, scritti in **Python** (stdlib only, nessuna dipendenza esterna).

---

## File

| File | Descrizione |
|---|---|
| `main.py` | Suite di test E2E completa (flusso realistico utente → appuntamento → referto → pagamento → log) |
| `stress_test.py` | Stress test parallelo che misura i tempi di risposta per utenti, availability e appuntamenti |
| `stress_v2.py` | Stress test avanzato con statistiche dettagliate su latenza e throughput |
| `test_hello_world.py` | Test di base per verificare la raggiungibilità del servizio |

---

## Prerequisiti

- **Python 3.7+**
- Medaryon in esecuzione su `http://localhost:3000` (oppure imposta la variabile `MEDARYON_BASE_URL`)

---

## Variabili d'ambiente

| Variabile | Default | Descrizione |
|---|---|---|
| `MEDARYON_BASE_URL` | `http://localhost:3000` | URL base dell'API Medaryon |
| `NUM_USERS` | `50` | Numero di utenti paralleli nello stress test (`stress_test.py`) |
| `CONCURRENCY` | `1000` | Numero di worker paralleli in `stress_v2.py` |
| `REQUESTS` | `10000` | Numero totale di richieste in `stress_v2.py` |

---

## Esecuzione

### Test E2E (`main.py`)

Esegue un flusso completo: registrazione, login, availability, prenotazione appuntamento, referto, pagamento e log.

```bash
cd python_test_e2e
python main.py
```

Con URL personalizzato:

```bash
MEDARYON_BASE_URL=http://localhost:3000 python main.py
```

### Test base (`test_hello_world.py`)

```bash
python test_hello_world.py
```

### Stress test (`stress_test.py`)

Crea utenti, availability e appuntamenti in parallelo e misura i tempi medi di risposta.

```bash
python stress_test.py

# Con parametri personalizzati
NUM_USERS=100 python stress_test.py
```

### Stress test avanzato (`stress_v2.py`)

Esegue un alto numero di richieste concorrenti e stampa statistiche dettagliate (media, deviazione standard, min/max latenza, throughput).

```bash
python stress_v2.py

# Con parametri personalizzati
CONCURRENCY=500 REQUESTS=5000 python stress_v2.py
```

---

## Output atteso

Il test E2E stampa il risultato di ogni test case:

```
test_01_register_patient ... ok
test_02_register_doctor ... ok
test_03_login_patient ... ok
...
----------------------------------------------------------------------
Ran 18 tests in X.XXXs

OK
```

Lo stress test avanzato stampa statistiche per ogni categoria di test:

```
=== USERS CREATION ===
Doctor register: 0.0423s, login: 0.0312s

=== AVAILABILITY STRESS TEST ===
Requests: 10000
Concurrency: 1000
Total time: 12.34s
Mean latency: 0.0456s
Throughput: 810.37 req/s
Errors: 0
```

---

## Note

- I test E2E sono pensati per essere eseguiti in sequenza (ogni test dipende dallo stato del precedente).
- I test di stress creano dati reali nel database; usali su un ambiente di test dedicato.
- In caso di errore di connessione, verifica che Medaryon e i side services siano in esecuzione.
