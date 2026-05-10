# Istruzioni per Gemini Code Assist (Agent Mode)

## 🎯 Obiettivo del Progetto
Questo repository (`listed-only-gcs-function`) contiene il codice e l'infrastruttura per un servizio sicuro di esplorazione file (Listing) ospitato su Google Cloud Run. 
Il servizio si connette a un bucket Cloud Storage montato localmente tramite GCS FUSE ed espone **esclusivamente i metadati** (nome, dimensione, data) per ragioni di sicurezza. L'identità dell'utente viene recuperata tramite header IAP (Identity-Aware Proxy).

## 🛠️ Regole di Stile e Architettura del Codice
Quando generi o modifichi il codice, rispetta queste linee guida:

1. **Linguaggio e Framework:** 
   - Python 3.11+.
   - Framework web: Flask.
2. **Standard di Sicurezza (Zero Download):** 
   - L'applicazione DEVE rimanere "Listed-Only". Non generare MAI endpoint o pulsanti HTML che permettano il download dei file o l'accesso in lettura al contenuto dei file (`read()`).
   - L'identità utente si basa SEMPRE sull'header `X-Goog-Authenticated-User-Email` passato da IAP.
3. **Infrastruttura GCS FUSE:**
   - Il bucket Cloud Storage è montato fisicamente in `/mnt/gcs`. Tutte le operazioni di listing devono passare da file system standard (es. `os.scandir`), NON utilizzare le librerie API di Google Cloud Storage (`google-cloud-storage`).
4. **Formattazione e Typing:** 
   - Applica Type Hints sulle funzioni logiche (es. `def format_time(timestamp: float) -> str:`).
   - Segui lo standard `Black` (88 caratteri) e `isort`.
5. **Gestione Errori:**
   - In caso di errore nel parsing della directory (es. permessi mancanti o bucket non montato), intercetta l'eccezione e mostrala nell'interfaccia HTML senza far crashare il server Flask (ritornando sempre HTTP 200 con il template di errore renderizzato).

## ☁️ Infrastruttura as Code (Terraform)
Se ti viene richiesto di generare l'infrastruttura per questo progetto, genera codice **Terraform** che implementi:
1. **Cloud Run Service:**
   - Execution environment: `gen2` (necessario per GCS FUSE).
   - Annotation volume: `run.googleapis.com/execution-environment: gen2` e montaggio volume GCS FUSE.
   - Ingress: `internal-and-cloud-load-balancing` (Per essere protetto da IAP).
2. **Service Account:**
   - Un service account dedicato con permessi minimi: `roles/storage.objectViewer` sul bucket target.
3. **IAP & Load Balancer:**
   - Configurazione di Identity-Aware Proxy per blindare l'endpoint Cloud Run.

## 🧪 Testing
- I test unitari devono usare `pytest`.
- Simula l'esistenza del mount point `/mnt/gcs` usando la libreria `unittest.mock` (`patch('os.scandir')`).
- Simula le richieste HTTP in ingresso "mockando" gli header IAP per verificare che l'identità dell'utente venga estratta correttamente o inserita come 'Anonymous_User'.
- I test vanno salvati nella cartella `/tests/`.

## 📦 Gestione Dipendenze e Docker
- Aggiorna `requirements.txt` solo se aggiungi librerie strettamente necessarie (es. `Flask`, `gunicorn`).
- Il `Dockerfile` deve esporre la porta 8080 e avviare l'app tramite Gunicorn (non usare il server di sviluppo Flask).

## 📝 Documentazione
- Scrivi docstring per le funzioni ausiliarie.
- Commenta le sezioni legate a IAP o GCS FUSE per spiegare la scelta architetturale ai futuri developer.
