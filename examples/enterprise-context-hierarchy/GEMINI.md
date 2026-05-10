# Global Context: Listed-Only GCS Function

## 🎯 Visione Architetturale e Sicurezza
Questo progetto implementa un servizio sicuro di esplorazione file ospitato su Google Cloud Run.
L'applicazione DEVE rimanere rigorosamente "Listed-Only" (solo metadati, nessun download).

## 🛡️ Regole Globali (Da applicare ovunque)
1. **Sicurezza Zero-Trust:** Nessun endpoint deve esporre dati o consentire esfiltrazione. L'identità è demandata interamente a IAP (Identity-Aware Proxy).
2. **Lingua:** Tutto il codice, i nomi delle variabili e l'infrastruttura devono essere scritti in Inglese. I commenti esplicativi e la documentazione tecnica possono essere in Italiano.
3. **Gerarchia dei Contesti:** Le regole specifiche per codice, infrastruttura e testing sono delegate ai file `GEMINI.md` presenti nelle rispettive sottocartelle (`/src`, `/terraform`, `/tests`).
