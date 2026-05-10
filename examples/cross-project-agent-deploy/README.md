# Cross-Project Deployment con Gemini Code Assist (Agent Mode)

Questa guida documenta la best practice "Enterprise" per utilizzare Gemini Code Assist (Agent Mode) separando fisicamente l'ambiente di coding da quello di esecuzione (Running/Deploy).

## 🎯 Lo Scenario (Mente vs Braccia)

In contesti aziendali restrittivi, è spesso necessario mantenere separati i perimetri:
- **Coding Project (Esterno/Sandbox):** È il progetto GCP (spesso fuori dall'organizzazione aziendale principale) dove è ospitata la **Cloud Workstation**. Qui si utilizza l'IDE, si scaricano le repository e si usufruisce della licenza di Gemini Code Assist. L'Intelligenza Artificiale ("La Mente") ragiona e consuma risorse qui.
- **Running Project (Interno/Org):** È il progetto all'interno del perimetro di sicurezza aziendale (es. protetto da VPC Service Controls) dove l'infrastruttura (Terraform) o il codice devono essere effettivamente rilasciati ed eseguiti ("Le Braccia").

L'obiettivo è permettere all'Agent Mode di eseguire autonomamente comandi terminale (es. `terraform apply`) puntando al Running Project, **senza scaricare chiavi JSON vulnerabili** sulla Workstation esterna.

---

## 🏗️ Architettura di Sicurezza: Service Account Impersonation

Per attraversare il perimetro in sicurezza, si utilizza l'**Impersonation del Service Account Cross-Project**.

### Step 1: Configurazione IAM (Creazione del "Ponte")

1. **Nel Running Project (Interno):**
   - Crea un Service Account dedicato al deploy (es. `deployer-interno@running-project.iam.gserviceaccount.com`).
   - Assegna a questo SA i ruoli necessari per l'infrastruttura che l'Agente dovrà creare (es. `roles/run.admin`, `roles/storage.admin`).

2. **La delega di Impersonation:**
   - Vai sempre su IAM nel Running Project.
   - Concedi il ruolo **Service Account Token Creator** (`roles/iam.serviceAccountTokenCreator`) sulla risorsa `deployer-interno` all'account di servizio predefinito della tua Cloud Workstation esterna (es. `SA-Esterno`).
   - *Risultato:* La Cloud Workstation è ora autorizzata a generare token a breve scadenza per agire "indossando la maschera" dell'account interno.

---

## 🚀 Setup della Demo (Istruzioni per l'Agent Mode)

Prima di lanciare qualsiasi prompt in Agent Mode, prepara l'ambiente nel terminale integrato dell'IDE (sulla Cloud Workstation).

### Step 2: Istruire il Terminale (Le Braccia)
Nel terminale della Workstation, digita:

```bash
# Ordina alla CLI di impersonare il Service Account interno
gcloud config set auth/impersonate_service_account deployer-interno@running-project.iam.gserviceaccount.com

# Forza il contesto dei comandi gcloud sul progetto interno
gcloud config set project running-project
```

### Step 3: Istruire l'Agente (La Mente tramite `GEMINI.md`)
Affinché Gemini Code Assist scriva il codice IaC in modo corretto e usi i tool di terminale consapevolmente, aggiungi questa direttiva nel file `GEMINI.md` (o `terraform/GEMINI.md`) del tuo workspace:

```markdown
# Istruzioni di Deployment
**Deploy Target:** Tutte le risorse Terraform (es. il campo `project` nel `provider.tf`) e i comandi di deploy (`gcloud`) generati devono puntare ESPLICITAMENTE al progetto `[RUNNING-PROJECT-ID]`. 

**Terminale:** Assumi che il terminale sia già configurato tramite Service Account Impersonation. Genera i comandi standard (es. `terraform apply`) e chiedi SEMPRE conferma all'utente prima dell'esecuzione ("Approval Required"). Non utilizzare configurazioni "auto-approve" o "YOLO mode".
```

---

## 🚦 Risultato Finale del Workflow

1. **Gemini Code Assist** è loggato nell'IDE e consuma token sul *Coding Project* esterno.
2. Legge il `GEMINI.md` e pianifica l'infrastruttura per il *Running Project* interno.
3. Chiede l'approvazione umana (Human-in-the-Loop) per eseguire `terraform apply`.
4. Una volta approvato, esegue il comando nel terminale integrato.
5. Grazie alla configurazione `impersonate_service_account`, la richiesta attraversa i confini aziendali in totale sicurezza, rispettando le policy dell'Organizzazione (niente chiavi JSON salvate su disco).
