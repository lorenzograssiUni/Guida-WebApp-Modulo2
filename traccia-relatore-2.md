# Traccia Relatore 2

Relatore 2 presenta le **slide 4–6**: virtualizzazione, architettura del progetto e ambienti di esecuzione.

---

## Slide 4 – Virtualizzazione, VM e container

1. **Introduzione alla virtualizzazione**
   - "Per capire perché usiamo Docker, partiamo dalla virtualizzazione."
   - "Virtualizzare significa creare versioni virtuali di risorse fisiche – CPU, memoria, storage, rete – su una singola macchina fisica."[cite:29]

2. **Hypervisor e macchine virtuali (VM)**
   - "L'hypervisor, o Virtual Machine Monitor, è il software che gestisce le macchine virtuali."[cite:29]
   - "Distribuisce le risorse hardware tra le VM e offre un livello di astrazione sopra al server fisico."
   - "Le VM hanno vantaggi: isolamento forte, sistema operativo guest completo, ambienti di test molto controllati."[cite:29]

3. **Limiti delle VM e motivazione dei container**
   - "Le VM però hanno svantaggi: sovraccarico dovuto al sistema operativo guest, maggior consumo di risorse e tempi di avvio più lunghi."[cite:29]
   - "Qui entrano in gioco i container: condividono il kernel dell'host, isolano solo processo e dipendenze, consumano meno risorse e si avviano molto più velocemente."[cite:29]
   - "Per un progetto come il nostro, che deve essere leggero e facilmente avviabile in locale, i container sono la soluzione ideale."

---

## Slide 5 – Architettura logica di Split Mate

1. **Descrivere i tre layer principali**
   - "L'architettura di Split Mate è composta da tre layer principali: frontend, backend e database."[cite:30][cite:32]
   - "Frontend: applicazione React/Vite (cartella `frontend-gestione-spese`) che gestisce l'interfaccia utente."[cite:30]
   - "Backend: API ASP.NET Core (cartella `gestione-spese`) che espone endpoint REST per utenti, gruppi, spese, divisioni e riepiloghi."[cite:32]
   - "Database: SQLite, che memorizza i dati e viene utilizzato sia in locale che nel deployment su Azure."[cite:32]

2. **Concetto di backend stateless**
   - "Il backend è progettato come *stateless*: non memorizza lo stato delle sessioni in memoria locale."[cite:29][cite:32]
   - "Ogni richiesta legge e scrive i dati sul database, che è il punto centrale di persistenza."
   - "Questo approccio è quello che in teoria viene chiamato 'stateless con persistenza dello stato' ed è molto importante per la scalabilità orizzontale."[cite:29]

3. **Collegamento alla teoria**
   - "L'idea è che possiamo avere più istanze dell'API che rispondono contemporaneamente, tutte appoggiate allo stesso archivio di dati: struttura pronta per scaling e autoscaling."[cite:29]

---

## Slide 6 – Ambienti: locale Docker vs cloud

1. **Presentare i due ambienti**
   - "La stessa applicazione vive in due ambienti: sviluppo locale con Docker Compose e deploy online su cloud pubblico (Azure + Vercel)."[cite:30]

2. **Ambiente locale Docker**
   - "In locale, abbiamo due container: uno per il backend ASP.NET Core e uno per il frontend React."[cite:30]
   - "Il file `docker-compose.yml` descrive questi servizi, le porte esposte e il volume per il database."[cite:30][cite:32]

3. **Ambiente cloud pubblico**
   - "Online, il backend è pubblicato su Azure App Service, quindi gira come app PaaS gestita da Azure."[cite:44]
   - "Il frontend è distribuito su Vercel, che si occupa di build, hosting e CDN."[cite:45][cite:30]

4. **Coerenza tra ambienti tramite configurazioni**
   - "Per rendere coerente il comportamento tra questi ambienti usiamo diverse configurazioni:"[cite:32]
   - "File `appsettings.json`, `appsettings.Development.json` e `appsettings.Docker.json` per gestire connection string e impostazioni a seconda dell'ambiente."[cite:32]
   - "Variabili di ambiente `ASPNETCORE_ENVIRONMENT` e `DOTNET_ENVIRONMENT` che dicono all'app se sta girando in Docker o in produzione."[cite:32]
   - "Variabile `VITE_API_URL` che permette al frontend di sapere quale URL di API chiamare (localhost in Docker, URL Azure nel deploy)."[cite:30][cite:41]

5. **Passaggio al relatore 3**
   - "Ora che abbiamo visto la struttura logica e i due ambienti, il relatore successivo entra nel dettaglio dei container: Dockerfile del backend, Dockerfile del frontend e il ruolo di Docker Compose."
