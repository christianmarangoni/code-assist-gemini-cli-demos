# Context: Unit & Integration Testing

## 🧪 Regole per i Test
Questa cartella contiene la suite di test per l'applicazione.

1. **Framework:** Usa ESCLUSIVAMENTE `pytest` e `pytest-mock`.
2. **Mocking Strategico:**
   - Non tentare mai di leggere file reali. Mokka sempre `os.scandir` e `os.path.exists`.
   - Simula l'autenticazione IAP iniettando finti header `X-Goog-Authenticated-User-Email` nei test del client Flask.
3. **Scenari Obbligatori:**
   - Testa il comportamento quando la directory `/mnt/gcs` è vuota.
   - Testa la risposta quando l'header IAP è assente o malformato.
   - Testa la corretta formattazione della dimensione file (byte formatting) e delle date.
4. **Coverage:** Punta a un Code Coverage del > 90%. Se scrivi nuovi test, assicurati che coprano i rami `except`.
