# Context: Application Code (Python/Flask)

## 🛠️ Regole di Sviluppo
Questa cartella contiene il codice sorgente dell'applicazione (`main.py`).

1. **Stack Tecnologico:** Python 3.11+, Flask.
2. **Standard di Codifica:**
   - Applica Type Hints obbligatori (`-> str`, `: dict`).
   - Usa `black` per la formattazione e `isort` per gli import.
3. **Gestione File System (GCS FUSE):**
   - Usa solo la libreria standard `os` (es. `os.scandir`) per leggere dal mount point `/mnt/gcs`.
   - **VIETATO** importare o usare `google-cloud-storage` (per evitare bypass del FUSE).
4. **IAP Headers:**
   - L'identità deve essere estratta SOLO dall'header `X-Goog-Authenticated-User-Email`.
5. **Error Handling:**
   - Non usare mai `try/except` vuoti. Intercetta gli errori di I/O e loggali con la libreria `logging` (formato JSON), ritornando un HTTP 200 con il template HTML che mostra un messaggio user-friendly.
