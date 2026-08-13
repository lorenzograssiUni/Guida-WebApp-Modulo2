# 04 - Architettura di deployment di Split Mate

Questo capitolo descrive come i concetti teorici di cloud e deployment sono stati concretamente applicati al progetto Split Mate, evidenziando la scelta di usare più provider e ambienti.[cite:29][cite:30]

## 4.1 Scelta dei modelli di servizio e deployment

In base alla classificazione NIST, il progetto utilizza:[cite:29][cite:30]

- Un **servizio PaaS** per il backend: l'API ASP.NET Core è pubblicata su **Azure App Service**, che gestisce automaticamente server, sistema operativo, runtime .NET, bilanciamento e scalabilità di base.
- Una piattaforma per applicazioni web stateless (SaaS/PaaS) per il frontend: l'app React/Vite è distribuita su **Vercel**, che si occupa di build, CDN, edge e routing.

Dal punto di vista del deployment, questa combinazione realizza uno scenario **multi‑cloud pubblico**: servizi diversi forniti da provider differenti (Microsoft Azure per il backend, Vercel per il frontend) ma integrati tramite HTTP e API.[cite:29][cite:30]

## 4.2 Ambienti: locale Docker e cloud pubblico

L'applicazione è pensata per operare in due ambienti principali:[cite:30][cite:32]

- **Ambiente locale Docker**: sviluppatori eseguono backend e frontend come container, utilizzando `docker-compose.yml` per orchestrare servizi, porte e volumi.
- **Ambiente cloud pubblico**: il backend è esposto da Azure App Service, mentre il frontend è servito da Vercel.

La coerenza tra questi ambienti è garantita da:

- Configurazioni di ambiente (`ASPNETCORE_ENVIRONMENT`, `DOTNET_ENVIRONMENT`) e `appsettings.*.json` per separare path del database e URL di ascolto.[cite:32]
- Variabile `VITE_API_URL` che permette al frontend di sapere dove chiamare l’API, sia in locale (Docker) sia online (Azure).[cite:30]

## 4.3 Gestione dello stato e persistenza dei dati

Dal punto di vista teorico, Split Mate implementa un **backend stateless con persistenza dello stato in un database**:[cite:29][cite:32]

- Le API non memorizzano dati di sessione nella memoria locale del server, rendendo più semplice la scalabilità orizzontale.
- Lo stato dell'applicazione (utenti, gruppi, spese, divisioni, riepiloghi) è salvato in un database (SQLite) che, in linea di principio, può essere sostituito in futuro con un DB gestito nel cloud.

Il codice `Program.cs` distingue il path del database tra:[cite:32]

- `/app/data/gestionespese.db` in ambiente Docker, con persistenza garantita dal volume `sqlite-data`.
- `C:\\home\\gestionespese.db` nel deployment su Azure App Service, sfruttando la cartella persistente di App Service per mantenere i dati tra deploy.

Questo pattern ricalca la teoria su **stateless con persistenza dello stato**: l'applicazione è progettata per poter scalare aggiungendo istanze dell'API, mantenendo il dato centrale in un archivio condiviso.[cite:29]
