# Relatore 4 — Slide 12–14

## Slide 12 — Gestione dello Stato per Architetture Scalabili

Concludiamo analizzando come Split Mate gestisce lo stato e quali sono le prospettive di scalabilità dell'architettura. Il backend è progettato come applicazione stateless: non conserva in memoria, all'interno di una singola istanza, le informazioni necessarie a riconoscere l'utente tra una richiesta e l'altra.

Attualmente l'autenticazione non è ancora implementata: il modello dati prevede un'entità Utente con email e password hash, ma non è configurato alcun meccanismo di login (JWT, cookie o Identity). Le richieste al backend sono quindi anonime; eventuali informazioni sull'utente sono gestite solo lato client e non vengono validate dal server. In un'evoluzione futura, introducendo JWT o cookie di sessione, il backend potrebbe validare le credenziali a ogni richiesta e ricavare da esse l'identità e le autorizzazioni dell'utente. Il vantaggio di un approccio basato su token è che ogni richiesta contiene le informazioni necessarie e può essere gestita da qualunque nodo backend, evitando le sticky session.

Lo stato applicativo di Split Mate non è conservato nella memoria del processo: utenti, gruppi, spese e bilanci vengono salvati nel database SQLite. In Docker il file del database è montato in /app/data/gestionespese.db, mentre in Azure e su Windows viene usato il percorso persistente C:\home\gestionespese.db, in modo da non dipendere dal filesystem temporaneo del container.

Questa separazione tra calcolo e stato è importante: il backend elabora le richieste, mentre il database conserva i dati. Tuttavia, bisogna essere precisi sui limiti della soluzione. Un volume o un percorso persistente protegge dalla perdita dovuta al riavvio del container, ma non equivale automaticamente a backup, replica o alta disponibilità.

Inoltre SQLite è un database basato su file. È semplice da usare e adeguato per un'applicazione con carico contenuto, ma la concorrenza e l'accesso simultaneo di molte istanze possono diventare un limite.

## Slide 13 — Prospettive di Scalabilità·°: Scale Out vs Scale Up

La slide sulla scalabilità distingue scale up e scale out.

Lo scale up, o scalabilità verticale, consiste nell'aumentare CPU e RAM dell'istanza Azure App Service esistente. È una soluzione semplice: non richiede di distribuire più copie dell'applicazione. Ha però un limite fisico e di piano, può avere costi maggiori e non elimina il rischio che l'unica istanza diventi indisponibile.

Lo scale out, o scalabilità orizzontale, consiste invece nell'aggiungere più istanze del backend dietro un load balancer. Il traffico viene distribuito tra i nodi con algoritmi come Round Robin, che alterna le richieste, oppure Least Connections, che preferisce l'istanza con meno connessioni attive.

L'architettura di Split Mate è di fatto stateless dal punto di vista del backend: non c'è sessione in memoria legata a una specifica istanza e tutte le informazioni persistenti (utenti, gruppi, spese, bilanci) sono salvate nel database SQLite. Attualmente non è implementata alcuna autenticazione tramite JWT o cookie, quindi non c'è validazione di token tra le richieste; questo rende teoricamente possibile indirizzare una richiesta a qualunque istanza senza perdere “stato di sessione”.

Il vero collo di bottiglia per uno scale out reale è SQLite: più nodi che accedono allo stesso file di database non equivalgono automaticamente a un database distribuito e concorrenziale. SQLite è adatto a carichi contenuti e a un'architettura con una singola istanza, ma non è progettato per essere condiviso in modo efficiente da molte istanze in parallelo.

Per abilitare un vero scale out dovremmo migrare SQLite verso un servizio gestito come PostgreSQL o Azure SQL Database. Un database di questo tipo è progettato per gestire accessi concorrenti, connessioni da più istanze e funzionalità di disponibilità e backup più complete. La migrazione richiederebbe anche aggiornare la connection string, applicare le migrazioni dello schema, configurare in modo sicuro le credenziali di accesso (ad esempio tramite i segreti di Azure App Service) e verificare che le prestazioni siano adeguate al carico atteso.

## Slide 14 — Il Blueprint Architetturale (Sintesi del Sistema)
La slide finale riassume il blueprint architetturale di Split Mate.
Lo sviluppo parte in locale: il team modifica frontend, backend e configurazioni Docker. Le modifiche sono versionate con Git tramite commit, feature branch e Pull Request. GitHub Actions esegue una pipeline CI che fa restore, build e validazione di codice e immagini Docker, impedendo che versioni non funzionanti arrivino in produzione. Docker e Docker Compose containerizzano backend e frontend, rendendo l’ambiente riproducibile. Il deployment è multi-cloud: Vercel ospita il frontend React, Azure App Service esegue le API ASP.NET Core; le variabili d’ambiente collegano i componenti e permettono di cambiare configurazione senza toccare il codice.

Il risultato è un percorso chiaro dal codice locale a un’infrastruttura distribuita e automatizzata. Split Mate non è solo “un’app caricata su Internet”: è un progetto in cui architettura, container, controllo versione, pipeline CI e cloud lavorano insieme, applicando in modo concreto principi cloud-native e 12-Factor (configurazione separata dal codice, build automatizzate, servizi containerizzati, backend stateless).

Per trasformare Split Mate in una vera web app aziendale servirebbero, tra le altre:

Autenticazione e autorizzazione complete
Attualmente il backend non ha autenticazione: esiste il modello Utente con email e password hash, ma non ci sono login, JWT, cookie di sessione o Identity. Servirebbe implementare login/logout, gestione sicura delle password, sessioni o token e ruoli/permessi (es. chi può creare/modificare/eliminare gruppi e spese).

Database scalabile e gestito
Tutto lo stato è su SQLite (gestionespese.db), montato in /app/data in Docker o in C:\home in Azure. SQLite è adatto a carichi contenuti, ma non a molte istanze concorrenti. Servirebbe migrare verso un servizio gestito (es. PostgreSQL o Azure SQL Database) con backup, replica e alta disponibilità.

Sicurezza rafforzata
Con un backend esposto su Internet servirebbero HTTPS obbligatorio, gestione sicura delle connection string e delle credenziali (segreti in Azure App Service o Key Vault), protezione da attacchi comuni (XSS, CSRF, SQL injection) e logging di sicurezza.

Monitoraggio e osservabilità
Strumenti di logging centralizzato, metriche di prestazioni e disponibilità e alerting in caso di errori o picchi di carico per capire cosa succede tra frontend, backend e database.

CI/CD più matura
Le pipeline attuali fanno build e validazione di codice e immagini Docker. Per la produzione servirebbero test automatici (unitari, di integrazione, end-to-end), ambienti separati (dev, staging, production) e strategie di rilascio controllato con rollback rapido.

Gestione errori e resilienza
Pagine di errore utente-friendly, gestione elegante di timeout e fallimenti del database e meccanismi di retry per aumentare la robustezza del sistema.

Con queste evoluzioni, Split Mate passerebbe da “progetto universitario ben strutturato” a “applicazione web pronta per un contesto aziendale”, mantenendo la stessa architettura di base ma rendendola sicura, scalabile e gestibile in produzione.
