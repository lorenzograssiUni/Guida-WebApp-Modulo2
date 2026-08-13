# Traccia Relatore 3

Relatore 3 presenta le **slide 7–9**: container Docker del backend, container del frontend e orchestrazione con Docker Compose.

---

## Slide 7 – Container backend: Dockerfile e gestione DB

1. **Introduzione al Dockerfile backend**
   - "Vediamo ora come abbiamo containerizzato il backend ASP.NET Core."
   - "Nel file `gestione-spese/Dockerfile` definiamo l'immagine Docker del backend."[cite:32]

2. **Struttura principale del Dockerfile**
   - "Usiamo un'immagine base .NET: prima lo SDK per compilare, poi il runtime per eseguire l'app."[cite:32]
   - "Copiamo il codice del progetto nella cartella `/app` dentro l'immagine."[cite:32]
   - "Eseguiamo `dotnet restore` e `dotnet publish` all'interno del container, usando `NuGet.Config` per controllare le fonti dei pacchetti."[cite:32][cite:41]
   - "L'entrypoint è `dotnet gestione-spese.dll`, che avvia l'API sulla porta configurata (5207)."[cite:32]

3. **Gestione del database in Program.cs**
   - "Nel file `Program.cs` abbiamo una logica per scegliere il path del database in base all'ambiente."[cite:32]
   - "Se l'ambiente è `Docker`, il path è `/app/data/gestionespese.db`; se è produzione su Azure, il path è `C:\\home\\gestionespese.db`."[cite:32][cite:44]
   - "Questo ci permette di usare la stessa API sia in container con volume montato, sia su App Service con storage persistente."

4. **Volume sqlite-data in docker-compose.yml**
   - "Nel file `docker-compose.yml` montiamo un volume `sqlite-data` sulla directory `/app/data` del container backend."[cite:30]
   - "In pratica il file SQLite vive fuori dall'immagine in un volume, e questo garantisce persistenza anche se il container viene ricreato."[cite:30]
   - "Questo è esattamente il pattern teorico 'stateless con persistenza dello stato esterno'."[cite:29]

---

## Slide 8 – Container frontend: build React/Vite e VITE_API_URL

1. **Descrizione del Dockerfile frontend**
   - "Per il frontend React/Vite abbiamo un Dockerfile separato nella cartella `frontend-gestione-spese`."[cite:30]
   - "La struttura tipica è: fase Node per installare le dipendenze e costruire il bundle, fase server statico per servire i file."[cite:30]

2. **Build della SPA e server statico**
   - "Durante la build eseguiamo `npm install` e `npm run build` per generare i file statici della Single Page Application."[cite:30]
   - "Poi copiamos il risultato dentro un'immagine base (ad esempio nginx) che espone il sito sulla porta 80 interna."[cite:30]

3. **Configurazione VITE_API_URL in docker-compose.yml**
   - "In `docker-compose.yml`, nella sezione frontend, passiamo un build arg `VITE_API_URL` al Dockerfile."[cite:30]
   - "In locale Docker questo valore è `http://localhost:5207/api`, cioè l'URL dell'API backend sulla macchina host."[cite:30]
   - "In produzione, quando usiamo Vercel, `VITE_API_URL` viene impostata all'URL dell'App Service Azure, così il frontend sa sempre dove chiamare l'API."[cite:30][cite:41]

4. **Collegamento alla teoria**
   - "Questo mostra come un'applicazione stateless front‑end possa essere spostata tra ambienti diversi cambiando solo una variabile di configurazione, in linea con i principi di cloud e deployment multi‑ambiente."[cite:29]

---

## Slide 9 – Orchestrazione con Docker Compose

1. **Presentare il file docker-compose.yml**
   - "Mettiamo insieme backend e frontend con `docker-compose.yml` nella root della repo."[cite:30]
   - "Qui definiamo i servizi, le porte, i volumi e le variabili di ambiente per l'ambiente Docker."[cite:30]

2. **Servizi e porte**
   - "Servizio `backend`: espone la porta `5207:5207`, monta il volume `sqlite-data:/app/data` e imposta `DOTNET_ENVIRONMENT` e `ASPNETCORE_ENVIRONMENT` su `Docker`."[cite:30][cite:32]
   - "Servizio `frontend`: costruisce l'immagine dalla cartella `frontend-gestione-spese`, espone la porta `3000:80` verso l'host."[cite:30]

3. **Healthcheck e depends_on**
   - "Sul backend definiamo un healthcheck con `curl -f http://localhost:5207/` per verificare che l'API sia attiva."[cite:30]
   - "Il frontend usa `depends_on` con `condition: service_started` per coordinare l'avvio con il backend, senza bloccare se l'healthcheck ritarda."[cite:30]

4. **Flusso di avvio per lo sviluppo**
   - "In sviluppo la sequenza tipica è: `docker compose down` per pulire, poi `docker compose up --build` o `docker compose up -d` per avviare in modalità detached."[cite:30]
   - "Alla fine abbiamo: backend attivo su `http://localhost:5207` e Swagger; frontend sul `http://localhost:3000`."

5. **Passaggio al relatore 4**
   - "Abbiamo visto come Docker ci permette di avere un ambiente locale molto vicino alla produzione. Ora il relatore successivo mostra come abbiamo automatizzato la build e il deploy con le pipeline GitHub Actions e il deployment multi‑cloud su Azure e Vercel."
