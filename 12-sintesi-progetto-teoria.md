# 12 - Sintesi del progetto e collegamento alla teoria

Questo capitolo conclude la guida, riassumendo come il progetto Split Mate integra i concetti teorici di cloud computing, virtualizzazione, containerizzazione e CI/CD, con particolare attenzione alle parti di interesse per l'esposizione (deployment, container Docker, pipeline).[cite:29][cite:30][cite:32][cite:41][cite:42]

## 12.1 Riepilogo degli elementi chiave

Gli elementi principali emersi nei capitoli precedenti sono:

- **Cloud computing secondo NIST**: definizione, caratteristiche essenziali, modelli di servizio (IaaS, PaaS, SaaS) e modelli di deployment (pubblico, privato, ibrido, multi‑cloud, community).[cite:29]
- **Virtualizzazione e container**: ruolo dell'hypervisor, differenze tra VM e container, vantaggi dei container in termini di leggerezza e tempi di avvio.[cite:29]
- **Architettura di Split Mate**: separazione tra frontend React/Vite, backend ASP.NET Core e database SQLite, con ambienti locale Docker e cloud pubblico.[cite:30][cite:32]
- **Container Docker**: Dockerfile backend e frontend, configurazioni `appsettings.*`, gestione del database tramite volume e path differenziati.[cite:30][cite:32]
- **Orchestrazione con Docker Compose**: definizione dei servizi, healthcheck, dipendenze, volumi e flusso di avvio locale.[cite:30]
- **Pipeline CI/CD con GitHub Actions**: workflow `CI - Build & Test` per build backend/frontend e workflow `Docker - Build Check` per la verifica delle immagini Docker e della sintassi Compose.[cite:41][cite:42]
- **Deployment multi‑cloud**: backend su Azure App Service (PaaS) e frontend su Vercel (piattaforma per web stateless), in uno scenario multi‑cloud pubblico.[cite:29][cite:30]
- **Gestione dello stato e scalabilità**: architettura stateless con persistenza dello stato in un database esterno, richiamo ai concetti di sticky sessions, sharding, autoscaling e cold start.[cite:29][cite:32]

## 12.2 Coerenza tra teoria e progetto

Il progetto Split Mate è stato progettato in modo da essere un esempio concreto dei concetti trattati nel modulo 2:

- La scelta di utilizzare **Docker** per lo sviluppo locale mostra la differenza tra esecuzione su VM tradizionali e su container leggeri, evidenziando i vantaggi descritti nella teoria.[cite:29][cite:30][cite:32]
- Il deployment su **Azure App Service** e **Vercel** illustra l'uso di servizi PaaS/SaaS in un contesto di cloud pubblico e multi‑cloud, collegando direttamente le definizioni NIST a un caso reale.[cite:29][cite:30]
- Le pipeline **GitHub Actions** dimostrano l'applicazione pratica dei principi DevOps e CI/CD, garantendo che ogni modifica venga automaticamente verificata e che le immagini Docker siano sempre costruibili.[cite:41][cite:42]

Questa coerenza rende il progetto un buon candidato per un'esposizione che non si limiti al codice, ma mostri consapevolezza delle scelte architetturali e di deployment.

## 12.3 Suggerimenti per l'esposizione orale

Per l'esposizione al docente, è utile seguire un percorso che tocchi in sequenza:

1. **Introduzione al progetto** e obiettivi didattici (cap. 1).
2. **Richiamo teorico essenziale**: definizione NIST, modelli di servizio/deployment, virtualizzazione e container (capp. 2–3).[cite:29]
3. **Architettura e ambienti del progetto**: locale Docker vs cloud (cap. 4).[cite:30][cite:32]
4. **Container Docker backend e frontend**: mostrare i Dockerfile, le configurazioni, i volumi e l'uso di `docker-compose` (capp. 5–7).[cite:30][cite:32]
5. **Pipeline CI/CD**: spiegare i workflow GitHub Actions, i badge e come garantiscono qualità e coerenza (cap. 8).[cite:41][cite:42]
6. **Deployment su Azure e Vercel**: raccontare il flusso di publish e l'integrazione multi‑cloud (capp. 9–10).[cite:30]
7. **Gestione dello stato e scalabilità**: collegare il design stateless del progetto ai concetti teorici di scaling e autoscaling (cap. 11).[cite:29][cite:32]

Seguendo questa traccia, l'esposizione metterà in evidenza proprio le parti di interesse del docente: **deployment**, **container Docker** e **pipeline**, sempre supportate dalla teoria del modulo.
