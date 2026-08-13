# 07 - Orchestrazione locale con Docker Compose

Questo capitolo illustra come backend e frontend vengono avviati insieme in ambiente locale tramite Docker Compose, collegando la pratica dell'orchestrazione multi‑container ai concetti teorici di deployment e gestione dello stato.[cite:29][cite:30]

## 7.1 Struttura di docker-compose.yml

Il file `docker-compose.yml` alla radice della repository `gestore-spese` definisce due servizi principali:[cite:30]

- `backend`: container ASP.NET Core con database SQLite.
- `frontend`: container React/Vite che serve l'interfaccia utente.

Per il backend sono configurati:

- Porta `5207:5207` per esporre l'API verso l'host.
- Volume `sqlite-data:/app/data` per la persistenza del database.
- Variabili di ambiente `DOTNET_ENVIRONMENT` e `ASPNETCORE_ENVIRONMENT` impostate su `Docker`.

Per il frontend sono configurati:

- Build context `./frontend-gestione-spese` e Dockerfile dedicato.
- Argomento di build `VITE_API_URL` per indicare l'indirizzo dell'API.
- Porta `3000:80` per esporre l'applicazione.
- `depends_on` per coordinare l'avvio con il backend.

La sezione `volumes` definisce il volume `sqlite-data`, utilizzato dal backend.[cite:30]

## 7.2 Healthcheck e dipendenze tra servizi

Nel servizio backend è definito un **healthcheck**:[cite:30]

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:5207/"]
  interval: 30s
  timeout: 10s
  retries: 3
```

Questo healthcheck permette di verificare che l'API sia raggiungibile prima di considerare il servizio "healthy". Il frontend usa `depends_on` con `condition: service_started` in modo che il suo avvio non sia bloccato da eventuali problemi di healthcheck, ma sia comunque coordinato con il backend.[cite:30]

Questi meccanismi di dipendenza richiamano le logiche di **gestione dello stato** e **scalabilità** viste nella teoria: un servizio può partire solo quando le sue dipendenze sono disponibili, e la salute di un'istanza influisce sul comportamento complessivo del sistema.[cite:29]

## 7.3 Flusso di avvio per lo sviluppo

Per avviare l'ambiente locale con Docker Compose, la sequenza tipica è:[cite:30]

1. Posizionarsi nella cartella della repository `gestore-spese`.
2. Eseguire `docker compose down` per fermare eventuali container precedenti.
3. Eseguire `docker compose up --build` per costruire le immagini e avviare i servizi.
4. In alternativa, usare `docker compose up -d` per avviare in modalità detached.

Una volta avviati i servizi:

- Il backend è raggiungibile su `http://localhost:5207` (e `http://localhost:5207/swagger`).
- Il frontend è raggiungibile su `http://localhost:3000`.

Questo setup permette di sviluppare, testare e debuggare l'applicazione in un contesto molto vicino a quello reale, ma completamente contenuto in macchina locale.

## 7.4 Debug e collegamento all'IDE

Per il backend ASP.NET Core, il debug può essere effettuato:

- Avviando il progetto direttamente dall'IDE (Visual Studio / VS Code) con il profilo Docker e collegando il debugger al processo nel container.
- Oppure eseguendo il backend fuori da Docker per semplificare la sessione di debug, mantenendo il database e il frontend nel container.

Queste scelte illustrano come la containerizzazione non sia solo un tema di deployment, ma anche di **esperienza di sviluppo**: l'uso di Docker nella fase di sviluppo rende più semplice riprodurre in locale problemi che potrebbero verificarsi in produzione.
