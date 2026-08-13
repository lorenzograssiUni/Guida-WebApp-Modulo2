# 08 - Pipeline CI/CD con GitHub Actions

Questo capitolo descrive le pipeline CI/CD definite nella repository `gestore-spese`, evidenziando il ruolo dei workflow **CI - Build & Test** e **Docker - Build Check** e il collegamento con i concetti teorici di automazione, qualità e deployment continuo.[cite:30][cite:41][cite:42]

## 8.1 Workflow "CI - Build & Test" (ci.yml)

Il file `.github/workflows/ci.yml` definisce un workflow che viene eseguito su ogni push e pull request verso il branch `main`.[cite:41]

Questo workflow ha due job principali:

- **Build Backend (.NET)**
- **Build Frontend (React/Vite)**

### 8.1.1 Job Backend (.NET)

Il job backend esegue i seguenti passi su un runner `ubuntu-latest`:[cite:41]

1. `actions/checkout@v4`: scarica il codice della repository.
2. `actions/setup-dotnet@v4`: installa .NET SDK 10.x.
3. `dotnet restore gestione-spese/gestione-spese.csproj --configfile gestione-spese/NuGet.Config`: ripristina le dipendenze del progetto usando il `NuGet.Config` locale per controllare le fonti dei pacchetti.[cite:32][cite:41]
4. `dotnet build gestione-spese/gestione-spese.csproj --no-restore --configuration Release`: compila il backend in configurazione Release.

Questo job assicura che il backend possa essere compilato correttamente in ambiente pulito, rilevando problemi di riferimento a pacchetti, errori di compilazione o incoerenze di configurazione.

### 8.1.2 Job Frontend (React/Vite)

Il job frontend esegue:[cite:41]

1. `actions/checkout@v4`: scarica il codice.
2. `actions/setup-node@v4`: installa Node.js 20 e configura la cache npm.
3. `npm ci` nella cartella `frontend-gestione-spese`: installa le dipendenze rispettando `package-lock.json`.
4. `npm run build` nella stessa cartella, con `VITE_API_URL` impostata all'URL dell'API Azure.

Questo garantisce che il frontend sia sempre compilabile e che il bundle possa essere generato correttamente per il deployment.

## 8.2 Workflow "Docker - Build Check" (docker.yml)

Il file `.github/workflows/docker.yml` definisce il workflow **Docker - Build Check**, che verifica la costruzione delle immagini Docker per backend e frontend e valida il file `docker-compose.yml`.[cite:42]

Il workflow è attivato su push e pull request che modificano Dockerfile, `docker-compose.yml` o il codice nelle cartelle `gestione-spese` e `frontend-gestione-spese`.[cite:42]

### 8.2.1 Job Docker Backend

Il job `docker-backend`:[cite:42]

1. Esegue `actions/checkout@v4`.
2. Configura Docker Buildx con `docker/setup-buildx-action@v3`.
3. Usa `docker/build-push-action@v6` per costruire l'immagine backend a partire da `gestione-spese/Dockerfile`.

L'immagine viene taggata come `splitmate-backend:ci` e non viene pushata (serve solo per verifica).[cite:42]

### 8.2.2 Job Docker Frontend

Il job `docker-frontend` è analogo ma opera sulla cartella `frontend-gestione-spese`:[cite:42]

1. Checkout del repository.
2. Setup Buildx.
3. Build dell'immagine frontend con `docker/build-push-action@v6`, passando il build arg `VITE_API_URL=http://localhost:5207/api`.[cite:42]

Anche qui l'immagine è taggata `splitmate-frontend:ci` e non viene pushata.

### 8.2.3 Job di validazione docker-compose

Il job `docker-compose-validate`:

1. Esegue il checkout.
2. Lancia `docker compose config --quiet`, che valida la sintassi e la configurazione del file `docker-compose.yml`.

Questo passaggio evita errori di configurazione che potrebbero impedire l'avvio dei container in ambiente locale.

## 8.3 Badge di stato nel README

Nel `README.md` della repository `gestore-spese` sono presenti due badge che mostrano lo stato dell'ultima esecuzione dei workflow:[cite:30]

- **CI - Build & Test**: indica se la build/test del backend e del frontend è passata o fallita.
- **Docker - Build Check**: indica se le immagini Docker e la configurazione Compose sono state costruite e validate correttamente.

Questi badge hanno una funzione didattica: permettono di vedere a colpo d'occhio se l'ultimo commit mantiene la qualità del software e la coerenza delle configurazioni di deployment.

## 8.4 Collegamenti teorici (DevOps e CI/CD)

Dal punto di vista teorico, le pipeline illustrano i principi di **CI/CD** descritti nel modulo 2:[cite:29]

- **Continuous Integration (CI)**: ogni commit viene automaticamente compilato e testato, riducendo il rischio di integrare codice rotto.
- **Quality Gates**: la build deve passare per poter considerare il codice pronto al deployment.
- **Automazione del deployment**: le pipeline Docker preparano le immagini pronte per essere eseguite sia in locale sia, potenzialmente, in un registro per il deployment su infrastruttura containerizzata.

Nel progetto Split Mate, queste pipeline fungono da ponte tra la teoria DevOps (automazione, qualità continua) e la pratica del corso, dimostrando come anche un progetto didattico possa adottare pratiche professionali di integrazione continua.
