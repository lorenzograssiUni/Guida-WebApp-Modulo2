# Traccia Relatore 4

Relatore 4 presenta le **slide 10–12**: pipeline CI/CD con GitHub Actions, workflow Docker e deployment multi‑cloud con gestione dello stato.

---

## Slide 10 – Workflow CI – Build & Test

1. **Introduzione al workflow CI**
   - "Ora vediamo come abbiamo automatizzato la build del progetto con GitHub Actions."
   - "Il file `.github/workflows/ci.yml` definisce il workflow *CI – Build & Test*, che parte ad ogni push o pull request sul branch `main`."[cite:41]

2. **Job Build Backend (.NET)**
   - "Il primo job è *Build Backend (.NET)*, eseguito su un runner `ubuntu-latest`."[cite:41]
   - "I passi principali sono:"
     - "Checkout della repository con `actions/checkout@v4`."
     - "Setup di .NET 10.x con `actions/setup-dotnet@v4`."
     - "`dotnet restore gestione-spese/gestione-spese.csproj --configfile gestione-spese/NuGet.Config` per ripristinare le dipendenze usando il nostro `NuGet.Config`."[cite:32][cite:41]
     - "`dotnet build` in configurazione Release, con `--no-restore` perché abbiamo già fatto il restore."[cite:41]
   - "Questo job garantisce che il backend si compili sempre correttamente in un ambiente pulito."

3. **Job Build Frontend (React/Vite)**
   - "Il secondo job è *Build Frontend (React/Vite)*."[cite:41]
   - "I passi principali sono:"
     - "Checkout della repository."
     - "Setup di Node.js 20 con `actions/setup-node@v4`, configurando la cache npm."[cite:41]
     - "`npm ci` in `frontend-gestione-spese` per installare le dipendenze in modo deterministico."[cite:41]
     - "`npm run build` nella stessa cartella, passando la variabile `VITE_API_URL` con l'URL dell'API Azure."[cite:41]
   - "In questo modo verifichiamo che anche il frontend sia sempre compilabile per il deploy."

4. **Collegamento DevOps**
   - "Questo workflow rappresenta la parte *Continuous Integration* della pratica DevOps: ogni commit viene verificato automaticamente, riducendo il rischio di integrare codice che non compila."[cite:29]

---

## Slide 11 – Workflow Docker – Build Check

1. **Scopo del workflow Docker**
   - "Oltre alla build standard, abbiamo un secondo workflow chiamato *Docker – Build Check* definito in `.github/workflows/docker.yml`."[cite:42]
   - "Serve per verificare che le immagini Docker del backend e del frontend si costruiscano correttamente e che il file `docker-compose.yml` sia valido."[cite:42]

2. **Job docker-backend**
   - "Il job `docker-backend` fa:"[cite:42]
     - "Checkout del repository."
     - "Setup di Docker Buildx con `docker/setup-buildx-action@v3`."
     - "Build dell'immagine backend usando `docker/build-push-action@v6` con context `./gestione-spese` e Dockerfile backend."[cite:42]
     - "Tag dell'immagine come `splitmate-backend:ci` senza pushare su un registro (serve solo per la verifica)."

3. **Job docker-frontend**
   - "Il job `docker-frontend` è analogo per il frontend:"[cite:42]
     - "Build dell'immagine frontend da `frontend-gestione-spese/Dockerfile`."
     - "Passaggio del build arg `VITE_API_URL=http://localhost:5207/api` per l'ambiente di test."[cite:42]

4. **Job docker-compose-validate**
   - "Infine abbiamo il job `docker-compose-validate`:"
     - "Esegue `docker compose config --quiet` per validare la sintassi e la configurazione di `docker-compose.yml`."[cite:42]
   - "In questo modo evitiamo di committare configurazioni Compose non valide che romperebbero l'ambiente Docker locale."

5. **Badge nel README**
   - "Nel README della repo principale compaiono due badge: uno per *CI – Build & Test* e uno per *Docker – Build Check*."[cite:30]
   - "A colpo d'occhio possiamo vedere se l'ultimo commit ha passato le build e le verifiche Docker."

---

## Slide 12 – Deployment multi-cloud e gestione dello stato

1. **Riassunto del deployment**
   - "Mettiamo insieme tutto quello che abbiamo detto sul deploy."
   - "Backend ASP.NET Core pubblicato su **Azure App Service** (PaaS): Azure gestisce infrastruttura, OS e runtime; noi gestiamo l'app e il DB in `C:\\home\\gestionespese.db`."[cite:44][cite:32]
   - "Frontend React/Vite distribuito su **Vercel**: piattaforma per web stateless con build automatica da GitHub."[cite:45][cite:30]

2. **Scenario multi-cloud pubblico**
   - "Il risultato è uno scenario multi‑cloud pubblico: usiamo servizi di due provider diversi (Microsoft Azure e Vercel) integrati via HTTP."[cite:29][cite:30]
   - "Questo è esattamente uno dei modelli di deployment visti nella teoria NIST."

3. **Gestione dello stato e scalabilità**
   - "Dal punto di vista dello stato, abbiamo scelto un'architettura *stateless con persistenza nel database*:"[cite:29][cite:32]
     - "Le API non memorizzano stato di sessione in memoria locale: ogni richiesta è indipendente."[cite:29]
     - "Tutti i dati sono salvati nel DB SQLite, che è l'unico punto di persistenza."[cite:32]
   - "Questo rende l'app pronta, in prospettiva, per scalare orizzontalmente aggiungendo istanze dell'API e migrare verso DB gestiti nel cloud (legati ai concetti di sharding, autoscaling e cold start visti nella teoria)."[cite:29][cite:46]

4. **Chiusura dell'esposizione**
   - "In conclusione, Split Mate è un esempio di come un progetto didattico possa integrare concretamente i concetti di cloud computing, container Docker e pipeline CI/CD, mantenendo coerenza tra teoria e pratica."
   - "Abbiamo mostrato sia la parte di deployment (locale e cloud) che quella di automazione, che sono esattamente i temi principali del modulo 2."
