# Gemini Code Assist & Gemini CLI - MCP Demos

Questa repository contiene le configurazioni e la documentazione per abilitare i **Server MCP (Model Context Protocol) Gestiti da Google Cloud**. 
L'obiettivo è trasformare Gemini Code Assist da un semplice "copilot" per il codice a un vero e proprio **SRE Agent autonomo**.

## Configurazione del `settings.json`

Per abilitare i poteri avanzati di Gemini Code Assist e Gemini CLI, è necessario configurare il file `settings.json` (situato in `~/.gemini/settings.json` o nella root di progetto).

Questo blocco JSON abilita l'autenticazione tramite Google Application Default Credentials (OAuth personale) e aggancia tutti i principali server MCP gestiti da Google Cloud (Developer Knowledge, Logging, GKE, Compute, Trace, Storage, Run e Cloud Assist).

```json
{
  "security": {
    "auth": {
      "selectedType": "oauth-personal"
    }
  },
  "mcpServers": {
    "gcp-developer-knowledge": {
      "httpUrl": "https://developerknowledge.googleapis.com/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    },
    "gcp-logging": {
      "httpUrl": "https://logging.googleapis.com/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    },
    "gcp-cloud-assist": {
      "httpUrl": "https://geminicloudassist.googleapis.com/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    },
    "gcp-gke": {
      "httpUrl": "https://container.googleapis.com/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    },
    "gcp-compute": {
      "httpUrl": "https://compute.googleapis.com/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    },
    "gcp-trace": {
      "httpUrl": "https://cloudtrace.googleapis.com/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    },
    "gcp-billing": {
      "httpUrl": "https://billingbudgets.googleapis.com/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    },
    "gcp-storage": {
      "httpUrl": "https://storage.googleapis.com/storage/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    },
    "gcp-run": {
      "httpUrl": "https://run.googleapis.com/mcp",
      "authProviderType": "google_credentials",
      "oauth": { "scopes": ["https://www.googleapis.com/auth/cloud-platform"] }
    }
  }
}
```

## Prerequisiti per il Funzionamento (IMPORTANTE)

1. **Abilitazione API:** Affinché l'agente non riceva un errore "Disconnected" o "Permission Denied", è fondamentale abilitare la **Developer Knowledge API** (e le API relative agli altri servizi che si vogliono interrogare) sul progetto Google Cloud collegato all'IDE.
2. **Reload IDE:** Dopo aver salvato il `settings.json`, eseguire sempre il "Reload Window" nell'IDE.
3. **Agent Mode:** Assicurarsi che il toggle "Agent" sia attivo nella barra laterale di Gemini Code Assist per permettergli di usare autonomamente i tool MCP.

## Link Ufficiali alla Documentazione Google Cloud MCP

- **Developer Knowledge MCP:** [https://developers.google.com/knowledge/api?hl=it](https://developers.google.com/knowledge/api?hl=it)
- **Autenticazione MCP:** [https://docs.cloud.google.com/mcp/authenticate-mcp](https://docs.cloud.google.com/mcp/authenticate-mcp)
- **Cloud Logging MCP:** [https://docs.cloud.google.com/logging/docs/reference/v2_mcp/mcp](https://docs.cloud.google.com/logging/docs/reference/v2_mcp/mcp)
- **Gemini Cloud Assist MCP:** [https://docs.cloud.google.com/gemini/docs/geminicloudassist/reference/mcp?hl=en](https://docs.cloud.google.com/gemini/docs/geminicloudassist/reference/mcp?hl=en)
- **Kubernetes Engine (GKE) MCP:** [https://docs.cloud.google.com/kubernetes-engine/docs/reference/mcp](https://docs.cloud.google.com/kubernetes-engine/docs/reference/mcp)
- **Compute Engine MCP:** [https://docs.cloud.google.com/compute/docs/reference/mcp](https://docs.cloud.google.com/compute/docs/reference/mcp)
- **Cloud Trace MCP:** [https://docs.cloud.google.com/trace/docs/reference/mcp/mcp](https://docs.cloud.google.com/trace/docs/reference/mcp/mcp)
- **Billing/Budget MCP:** [https://docs.cloud.google.com/billing/docs/reference/budget/rest](https://docs.cloud.google.com/billing/docs/reference/budget/rest)
- **Cloud Storage MCP:** [https://docs.cloud.google.com/storage/docs/reference/mcp](https://docs.cloud.google.com/storage/docs/reference/mcp)
- **Cloud Run MCP:** [https://docs.cloud.google.com/run/docs/reference/mcp](https://docs.cloud.google.com/run/docs/reference/mcp)
