# 01 - Introduzione a Split Mate – Gestore Spese di Gruppo

Questo capitolo introduce il progetto sviluppato nel modulo 2, una web application full‑stack per la gestione e la divisione delle spese di gruppo, chiamata **Split Mate – Gestore Spese di Gruppo**.[cite:30]

## 1.1 Contesto didattico e obiettivi del progetto

Il progetto nasce all'interno del corso WebApp del modulo 2 come applicazione di riferimento per ripassare e applicare in pratica i concetti teorici di cloud computing, virtualizzazione, containerizzazione e CI/CD illustrati nel materiale del prof. Lillini.[cite:29]

Gli obiettivi principali sono:

- Progettare e realizzare una web application completa (frontend + backend + database) con tecnologie moderne.
- Sperimentare la distribuzione dell'applicazione in più ambienti: sviluppo locale, container Docker e cloud pubblico.
- Collegare ogni scelta tecnica (architettura, deployment, pipeline) ai concetti teorici di NIST, modelli di servizio e modelli di deployment.

## 1.2 Architettura logica del sistema

Split Mate è composto da tre macro‑componenti:[cite:30][cite:32]

- **Frontend**: applicazione React/Vite, responsabile dell'interfaccia utente e della gestione delle interazioni dei membri del gruppo.
- **Backend API**: applicazione ASP.NET Core che espone le API REST per la gestione di utenti, gruppi, spese, divisioni e riepiloghi.
- **Database**: archivio persistente dei dati (in questo progetto SQLite), utilizzato sia in locale con Docker sia nel deployment online.

L'architettura è progettata per essere **stateless lato applicazione**: le API non mantengono stato di sessione in memoria locale, ma delegano la persistenza a un database. Questo rende più semplice collegare il progetto ai concetti di scalability, autoscaling e gestione dello stato (stateful/stateless) descritti nel materiale teorico.[cite:29]

## 1.3 Ambienti di esecuzione

La stessa applicazione è pensata per funzionare in due contesti principali:

- **Sviluppo locale con Docker Compose**: il backend e il frontend girano come container, orchestrati da `docker-compose.yml`, con un volume per la persistenza del database SQLite.[cite:30][cite:32]
- **Deploy online disconnesso da Docker**: il backend è pubblicato su Azure App Service (modello PaaS) e il frontend su Vercel (piattaforma per web stateless), realizzando uno scenario multi‑cloud coerente con i modelli di deployment NIST.[cite:30]

Questa doppia modalità permette di mostrare in esposizione sia la **progettazione dei container** che il **deployment su cloud**, mantenendo una forte correlazione con la teoria sul cloud pubblico, IaaS/PaaS/SaaS, virtualizzazione e CI/CD.
