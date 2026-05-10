# Context: Infrastructure as Code (Terraform)

## ☁️ Regole di Provisioning
Questa cartella contiene i file `.tf` per il deploy su Google Cloud.

1. **Provider e Versioning:**
   - Usa sempre il provider `google` e `google-beta` versione `> 5.0.0`.
   - Usa blocchi `terraform {}` con `required_version`.
2. **Configurazione Cloud Run (Critica):**
   - L'ambiente di esecuzione DEVE essere `gen2` (richiesto per GCS FUSE).
   - Ingress DEVE essere settato su `internal-and-cloud-load-balancing`.
   - Aggiungere il blocco per il mount del volume GCS FUSE.
3. **Sicurezza IAM (Principio del Minimo Privilegio):**
   - Il Service Account di Cloud Run deve avere ESCLUSIVAMENTE il ruolo `roles/storage.objectViewer` sul bucket specifico, mai a livello di intero progetto.
4. **Variabili e Output:**
   - Tutte le variabili in `variables.tf` devono avere `type` e `description`.
