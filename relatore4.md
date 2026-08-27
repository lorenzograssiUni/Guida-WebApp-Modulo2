# Relatore 4 — Slide 12–14

## Testo da studiare

Concludiamo analizzando come Split Mate gestisce lo stato e quali sono le prospettive di scalabilità·°dell'architettura. Il backend è progettato come applicazione stateless, cioè non conserva localmente, nella memoria di una singola istanza, le informazioni necessarie a riconoscere l'utente tra una richiesta e l'altra.

L'autenticazione viene gestita tramite token JWT. Dopo l'autenticazione, il client invia il token nelle richieste successive; il backend lo valida ogni volta e ricava da esso l'identità·°e le autorizzazioni dell'utente. Il vantaggio è che ogni richiesta contiene le informazioni necessarie e può essere gestita da qualunque nodo backend.

Questo evita la sticky session. Una sticky session obbligherebbe il load balancer a inviare sempre lo stesso utente alla stessa istanza, perché solo quella istanza conoscerebbe la sua sessione locale. Nel nostro modello non è necessario: se aggiungiamo più nodi backend, ciascuno può validare il JWT e accedere ai dati persistenti.

Lo stato applicativo di Split Mate non deve essere conservato nella memoria del processo. Utenti, gruppi, spese e bilanci vengono salvati nel database SQLite. In Azure il database è allocato nel percorso persistente `C:\home`, così non dipende esclusivamente dal filesystem temporaneo del container.

Questa separazione tra calcolo e stato è importante: il backend elabora le richieste, mentre il database conserva i dati. Tuttavia, bisogna essere precisi sui limiti della soluzione. Un volume o un percorso persistente protegge dalla perdita dovuta al riavvio del container, ma non equivale automaticamente a backup, replica o alta disponibilità.

Inoltre SQLite è un database basato su file. È semplice da usare e adeguato per un'applicazione con carico contenuto, ma la concorrenza e l'accesso simultaneo di molte istanze possono diventare un limite. Per questo la slide sulla scalabilità·°distingue scale up e scale out.

Lo scale up, o scalabilità·°verticale, consiste nell'aumentare CPU e RAM dell'istanza Azure App Service esistente. È una soluzione semplice: non richiede di distribuire più copie dell'applicazione. Ha però un limite fisico e di piano, può avere costi maggiori e non elimina il rischio che l'unica istanza diventi indisponibile.

Lo scale out, o scalabilità·°orizzontale, consiste invece nell'aggiungere più istanze del backend dietro un load balancer. Il traffico viene distribuito tra i nodi con algoritmi come Round Robin, che alterna le richieste, oppure Least Connections, che preferisce l'istanza con meno connessioni attive.

L'architettura stateless di Split Mate rende possibile lo scale out dal punto di vista del backend. Qualunque nodo può verificare il JWT e leggere o aggiornare i dati. Il vero collo di bottiglia, però, è SQLite: più nodi che accedono allo stesso database locale non equivalgono automaticamente a un database distribuito e concorrenziale.

Per abilitare un vero scale out dovremmo migrare SQLite verso un servizio gestito come PostgreSQL o Azure SQL Database. Un database di questo tipo è progettato per gestire accessi concorrenti, connessioni da più istanze e funzionalità di disponibilità e backup più complete. La migrazione richiederebbe anche aggiornare la connection string, applicare le migrazioni dello schema, configurare i segreti e verificare le prestazioni.

La slide finale riassume il blueprint architetturale. Il primo passaggio è lo sviluppo locale: il team modifica frontend, backend e configurazioni Docker. Il secondo è il versionamento con Git: le modifiche vengono salvate nei commit e proposte tramite feature branch e Pull Request.

Il terzo passaggio è la CI/CD con GitHub Actions. I workflow eseguono restore, build e validazione, impedendo che codice o immagini non funzionanti arrivino all'integrazione. Il quarto è la containerizzazione con Docker: i Dockerfile multi-stage producono immagini riproducibili per backend e frontend, mentre Docker Compose descrive lo stack locale.

Il quinto passaggio è il deployment multi-cloud. Vercel pubblica il frontend React e Azure App Service esegue le API ASP.NET Core. Le variabili d'ambiente collegano correttamente i componenti e permettono di cambiare configurazione senza modificare il codice.

Il risultato è il percorso dal codice locale a un'infrastruttura distribuita e automatizzata. Split Mate non è semplicemente un'applicazione caricata su Internet: è un progetto in cui architettura applicativa, container, controllo versione, pipeline e cloud collaborano.

Possiamo concludere dicendo che Split Mate applica i principi cloud-native e 12-Factor in modo concreto. La configurazione è separata dal codice, le build sono automatizzate, i servizi sono containerizzati e il backend è progettato per non dipendere da sessioni locali. Allo stesso tempo riconosciamo i limiti attuali: SQLite è adatto alla dimensione del progetto, ma per un carico maggiore servirebbe un database gestito e più adatto alla concorrenza.

Questa consapevolezza è importante perché dimostra che non stiamo soltanto usando parole teoriche: sappiamo spiegare quali scelte sono state fatte, quali vantaggi producono e quale modifica sarebbe necessaria per portare Split Mate a una scala più grande.