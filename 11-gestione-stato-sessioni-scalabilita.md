# 11 - Gestione dello stato, sessioni e scalabilità nel progetto

Questo capitolo collega la teoria sulla **gestione dello stato**, sulle applicazioni stateful/stateless e sulle sessioni al modo in cui Split Mate gestisce i dati e le richieste, sia in locale con Docker sia nel deployment su cloud.[cite:29][cite:32]

## 11.1 Applicazioni stateful e stateless

Nel materiale del modulo 2 vengono distinti tre approcci principali:[cite:29]

- **Applicazioni stateful**: memorizzano dati di sessione localmente sul server (es. carrello spesa in RAM). Sono più difficili da scalare orizzontalmente perché l'utente deve restare agganciato alla stessa istanza.
- **Applicazioni stateless**: ogni richiesta è indipendente e il server non memorizza nulla localmente sull'utente. Sono ideali per la scalabilità orizzontale.
- **Stateless con persistenza dello stato**: l'applicazione non salva lo stato sul server applicativo, ma lo memorizza in un archivio esterno (database, cache distribuita), mantenendo l'architettura scalabile.[cite:29]

Split Mate segue quest'ultimo modello:

- Le API ASP.NET Core non memorizzano lo stato della sessione in memoria locale.
- I dati di utenti, gruppi, spese, divisioni e riepiloghi sono persistiti in un database SQLite.[cite:32]

Questo rende l'applicazione più semplice da scalare in futuro: aggiungendo nuove istanze dell'API, tutte possono accedere allo stesso archivio di dati.

## 11.2 Session persistence e sticky sessions (richiamo teorico)

Il materiale teorico introduce il concetto di **sticky sessions**, dove un load balancer assegna sempre lo stesso utente alla stessa istanza di server per mantenere lo stato in memoria locale.[cite:29]

Vantaggi:

- Compatibilità con applicazioni legacy che memorizzano lo stato sul server.

Svantaggi:

- Bilanciamento inefficiente del carico.
- Complessità nella gestione dell'identità degli utenti.

Nel progetto Split Mate, questa tecnica non è utilizzata direttamente, ma viene citata per mostrare come una progettazione stateless con database esterno sia preferibile in vista di possibili evoluzioni verso più istanze e autoscaling.

## 11.3 Scalabilità del database e sharding (richiamo teorico)

Il PDF WebApp Lillini introduce anche il concetto di **scalabilità orizzontale dei database (sharding)**, dove i dati vengono partizionati su più server indipendenti.[cite:29]

Caratteristiche:

- Scalabilità lineare: aumentando il numero di server, aumenta la capacità di gestione dei dati.
- Complessità delle query cross‑shard: operazioni che coinvolgono dati distribuiti su shard diversi possono essere lente.

Nel progetto attuale, Split Mate utilizza un singolo database SQLite. Tuttavia, la struttura delle entità (utenti, gruppi, spese) è pensata in modo da poter essere migrata in futuro verso un database gestito nel cloud che supporti tecniche di scaling, in linea con questi concetti teorici.[cite:32]

## 11.4 Autoscaling e tempi di avvio

Il materiale teorico discute l'**autoscaling**, ovvero la capacità di aumentare o diminuire automaticamente le risorse computazionali in base alla domanda effettiva, e il concetto di **cold start time** (tempo necessario per avviare una nuova istanza).[cite:29]

Nel caso di Split Mate:

- In ambiente locale Docker, l'avvio dei container backend e frontend ha un tempo limitato, ma non è gestito in modo automatico: lo sviluppatore avvia manualmente `docker compose up`.
- In ambiente Azure App Service e Vercel, la piattaforma può in futuro gestire autoscaling e cold start di nuove istanze dell'app, soprattutto se l'architettura rimane stateless.

La progettazione attuale (API stateless, frontend stateless, persistenza esterna) è coerente con i requisiti per sfruttare queste funzionalità avanzate di autoscaling in contesti cloud.
