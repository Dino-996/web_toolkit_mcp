# 🌐Web Toolkit MCP

Questo toolkit consente a modelli eseguiti in locale di accedere a Internet in modo sicuro, superando i limiti della conoscenza statica e recuperando contenuti aggiornati.

---

## 🛠️ Tool disponibili

Il server espone 4 strumenti principali tramite trasporto STDIO:

| Tool | Parametri | Descrizione |
| :--- | :--- | :--- |
| `web_search` | `query`, `max_results` (1-10), `region` | Esegue ricerche web tramite DuckDuckGo e restituisce titoli, URL ed estratti sintesi. |
| `read_webpage` | `url` | Scarica pagine web statiche, rimuove elementi inutili (script, menu, footer) e restituisce il testo pulito. |
| `parse_table` | `url`, `table_index` | Usa un browser headless Chromium (Playwright) per eseguire JavaScript ed estrarre tabelle HTML in formato strutturato. |
| `check_url_status` | `url`, `timeout` | Esegue un controllo rapido di raggiungibilità, restituisce il codice di stato HTTP, l'URL finale e la latenza in ms. |
---

## 🚀 Requisiti e installazione

1. **Prerequisiti:** Python 3.10 o superiore e Node.js installati nel sistema.
2. **Clona il repository e crea l'ambiente virtuale:**
	```bash
   git clone https://github.com/Dino-996/web_toolkit_mcp.git
   cd web_toolkit_mcp
   python -m venv .venv
   ```
4. **Attiva l'ambiente virtuale:** 
    ```powershell
    # Windows
    .\.venv\Scripts\activate
    ```
    ```bash
    # Linux \ macOS
    source .venv/bin/activate
    ```
5.  **Installa le dipendenze e i browser di Playwright:**
    ```bash
    pip install -r requirements.txt
    playwright install chromium
    ```

## ⚙️ Configurazione in LM Studio

Aggiungi il server all'interno del file `mcp.json` di LM Studio:

### Windows

```json
{
  "mcpServers": {
    "web-toolkit": {
      "command": "[percorso_assoluto_progetto]\\.venv\\Scripts\\python.exe",
      "args": [
        "[percorso_assoluto_progetto]\\main.py"
      ]
    }
  }
}
```
> **Nota per utenti Windows:** Assicurati di sostituire i percorsi con quelli assoluti del tuo sistema, usando il doppio backslash `\\` per l'escape.
> 
### Linux / macOS

```json
{
  "mcpServers": {
    "web-toolkit": {
      "command": "[percorso_assoluto_progetto]/.venv/bin/python",
      "args": [
        "[percorso_assoluto_progetto]/main.py"
      ]
    }
  }
}
```

## 🔒 Sicurezza e protezione

Il codice integra misure di sicurezza native per l'esecuzione sicura in ambienti locali:

-   **Protezione SSRF Hop-by-Hop:** Risoluzione manuale dei redirect per bloccare tentativi di navigazione verso IP privati.
    
-   **Mitigazione Prompt Injection:** I testi estrapolati dal web vengono racchiusi nei tag `[contenuto_non_attendibile]`, indicando all'LLM di trattarli come dati da analizzare e non come istruzioni da eseguire.

-   **Isolamento STDIO:** Tutti i log di sistema vengono indirizzati su `sys.stderr` per evitare interferenze con il canale di comunicazione JSON-RPC su `stdout`.

-   **Gestione Risorse:** Controllo delle istanze concorrenti di Chromium tramite semafori in thread separati per prevenire il saturamento della memoria RAM.
    

## 📚 Docs

### Documentazione MCP

Specifiche ufficiali del protocollo MCP:
`https://modelcontextprotocol.io`

### LM Studio Docs

Guida all'integrazione dei server MCP nell'interfaccia:
`https://lmstudio.ai/docs`
