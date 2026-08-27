# Relatore 2 — Slide 6–8

## Slide 6 — L'Ecosistema Docker: Multi-Stage Builds

Ora analizziamo come Split Mate viene containerizzato e avviato in locale. Il repository contiene un Dockerfile per il backend, nella cartella `gestione-spese`, e un Dockerfile per il frontend, nella cartella `frontend-gestione-spese`. L'obiettivo è creare immagini leggere, riproducibili e adatte a essere usate sia dallo sviluppatore sia dalla pipeline CI/CD.

Per il backend usiamo una build multi-stage. Nel primo stage partiamo dall'immagine .NET 10 SDK. Questa immagine contiene gli strumenti necessari per eseguire `dotnet restore`, risolvere i pacchetti NuGet, compilare il progetto e produrre gli artefatti con `dotnet publish`.

Il secondo stage parte invece dall'immagine ASP.NET Runtime. Non servono più tutti gli strumenti dell'SDK: copiamo soltanto gli artefatti pubblicati dal primo stage e definiamo l'entrypoint, cioè l'avvio di `gestione-spese.dll`. In questo modo l'immagine finale non contiene codice sorgente e strumenti di compilazione non necessari all'esecuzione.

Il vantaggio della multi-stage build è quindi duplice: separa compilazione ed esecuzione e riduce la dimensione e la superficie d'attacco dell'immagine. In produzione è buona pratica usare un'immagine runtime più piccola, concedere solo i permessi necessari e passare la configurazione tramite variabili d'ambiente.

Anche il frontend usa una build multi-stage. Nel primo stage utilizziamo Node 20 per installare le dipendenze ed eseguire `npm run build`, che produce gli asset della Single Page Application React, normalmente tramite Vite. Nel secondo stage utilizziamo Nginx, un web server leggero, per servire i file statici generati.

Nginx deve essere configurato con il fallback della SPA. Se l'utente naviga verso una rotta gestita da React e aggiorna la pagina, il server deve restituire `index.html`; sarà poi il router del frontend a interpretare la rotta. Senza questo fallback, un refresh su una pagina diversa dalla home potrebbe restituire un errore 404.

Nel processo di build è importante non copiare file inutili. Il `.dockerignore` esclude, per esempio, `node_modules` e altri artefatti locali. Le dipendenze vengono reinstallate dentro l'immagine usando il file di lock del progetto. Così l'immagine contiene solo ciò che serve e il risultato è più riproducibile.

## Slide 7 — Orchestrazione Locale (Docker Compose)

Passiamo alla slide sull'orchestrazione locale. Split Mate è formato da frontend, backend e database SQLite. Docker Compose descrive questi servizi in un unico file e permette di avviarli con il comando `docker compose up --build -d`.

Il frontend è esposto sulla porta 3000, mentre il backend ascolta sulla porta 5207. Le porte servono per rendere i servizi raggiungibili dall'host; all'interno della rete Docker Compose i container comunicano usando i nomi dei servizi. Questo è preferibile rispetto all'uso di indirizzi IP scritti manualmente, perché gli IP dei container possono cambiare.

Il database utilizzato è SQLite. Poiché·°il database contiene lo stato dell'applicazione, non possiamo lasciarlo soltanto nel filesystem temporaneo del container. Nel Compose viene usato il volume `sqlite-data:/app/data`: i dati vengono conservati nel volume anche se il container viene eliminato e ricreato.

Il volume protegge dalla ricreazione del container, ma non deve essere confuso con un sistema completo di backup. Per un ambiente cloud realmente affidabile servirebbero anche backup e una strategia di ripristino. Inoltre SQLite è adatto a un progetto contenuto, ma presenta limiti quando aumentano concorrenza e numero di istanze; questo sarà importante nella parte sulla scalabilità·°.

Per gestire il ciclo di vita dei servizi usiamo healthcheck e `depends_on`. L'healthcheck, eseguito tramite `curl`, verifica che il backend sia effettivamente pronto a rispondere. `depends_on` stabilisce l'ordine di avvio e, insieme al controllo di salute, evita che il frontend tenti di utilizzare un backend non ancora pronto. È importante distinguere l'ordine di avvio dalla disponibilità reale: un container avviato non è necessariamente un servizio pronto.

## Slide 8 — Cultura DevOps e Flusso di Lavoro Distribuito

L'ultima slide riguarda la cultura DevOps e il flusso di lavoro del team. DevOps non è soltanto uno strumento, ma un modo di collaborare in cui sviluppo, test e rilascio sono collegati. Il framework CALMS riassume Culture, Automation, Lean, Measurement e Sharing.

Nel nostro gruppo la Culture corrisponde alla responsabilità condivisa tra i quattro membri. Automation significa usare workflow CI/CD invece di verificare tutto manualmente. Lean significa lavorare su feature piccole e modulari. Measurement e Sharing significano tracciare il codice, condividere il lavoro e rendere visibili modifiche e risultati.

Usiamo Git come sistema di controllo versione distribuito. Ogni componente viene sviluppato su un feature branch e poi integrato in `main` tramite Pull Request. La protezione di `main` obbliga a revisionare le modifiche prima dell'integrazione e riduce il rischio di introdurre errori, per esempio nella logica di calcolo delle spese. Il prossimo relatore mostrerà·°come questo flusso attiva automaticamente i controlli di CI.
