# Relatore 1 — Slide 2–5

## Slide 2 — Dal Codice all'Infrastruttura

Buongiorno, presentiamo Split Mate, un'applicazione web per la gestione e la divisione delle spese di gruppo. Il progetto nasce nel primo modulo come web application full-stack composta da un frontend React, da un backend ASP.NET Core Web API e da un database SQLite. L'utente può registrare spese sostenute durante una cena, un viaggio o una convivenza e visualizzare il bilancio del gruppo, cioè quanto ogni partecipante deve pagare o ricevere.

## Slide 2 — L'Evoluzione (Modulo 2)

Nel secondo modulo il progetto viene fatto evolvere. Non ci limitiamo più a eseguire frontend, backend e database sul computer dello sviluppatore: trasformiamo l'applicazione in un sistema più vicino a un'architettura cloud-native. Questo significa containerizzare i componenti, automatizzare la compilazione e i controlli, e pubblicare frontend e backend su servizi cloud.

L'obiettivo è quindi passare dal semplice codice all'infrastruttura. Il codice applicativo rimane fondamentale, ma viene affiancato da Docker, Docker Compose, GitHub Actions e servizi cloud. In questo modo l'ambiente di sviluppo può essere riprodotto e il rilascio diventa più affidabile.

## Slide 4 — Modelli di Servizio: Multi-Cloud Pubblico

La prima scelta architetturale riguarda la collocazione dei componenti. Il frontend React è una Single Page Application: dopo la build produce file statici JavaScript, CSS e HTML che vengono serviti all'utente. Per questo viene pubblicato su Vercel, piattaforma adatta a frontend moderni e a deployment automatici collegati a GitHub.

Il backend ASP.NET Core espone API REST. Le API ricevono le richieste del frontend, applicano la logica di business relativa a utenti, gruppi e spese, e accedono al database per leggere o salvare i dati. Il backend viene ospitato su Azure App Service, che è un servizio PaaS: il provider gestisce il sistema sottostante e noi ci concentriamo sull'applicazione e sulla sua configurazione.

Questa distribuzione usa due provider cloud pubblici e per questo viene definita multi-cloud. Non significa duplicare inutilmente l'intero sistema, ma scegliere il servizio più adatto per ogni componente: Vercel per distribuire la SPA e Azure per eseguire le API .NET. Un ulteriore vantaggio è ridurre la dipendenza da un unico fornitore, anche se bisogna gestire con attenzione configurazione, networking e variabili d'ambiente.

Il collegamento tra frontend e backend avviene tramite HTTP e API REST. Il frontend deve sapere a quale indirizzo inviare le richieste; questo indirizzo non può essere sempre localhost, perché localhost indica il computer o il container dal quale parte la richiesta. In produzione l'indirizzo deve essere quello pubblico del backend su Azure.

## Slide 3 — Il Paradigma Cloud (NIST) Applicato

La slide sul paradigma cloud applica al nostro caso le caratteristiche definite dal NIST. La prima è l'on-demand self-service: il team può creare e configurare risorse tramite il portale del provider senza dover chiedere manualmente al personale di Azure o Vercel di installare ogni componente.

La seconda è l'ampio accesso alla rete. Il frontend su Vercel e le API su Azure vengono raggiunti via Internet, quindi l'applicazione non è legata al computer dello sviluppatore. La terza è il pooling delle risorse: App Service utilizza un'infrastruttura condivisa tra più clienti, mentre l'isolamento dei servizi nasconde la complessità·°hardware all'utente.

La quarta caratteristica è l'elasticità·°rapida. L'architettura è predisposta ad aumentare le risorse o il numero di istanze quando il traffico cresce, anche se nel nostro progetto il dimensionamento dipende dal piano Azure scelto. La quinta è il servizio misurato: il provider monitora l'uso delle risorse e il modello cloud consente di collegare il consumo al costo. In fase progettuale usiamo il piano gratuito Azure Free F1, quindi ottimizziamo le risorse disponibili.

## Slide 5 — Container vs Macchine Virtuali

Infine, la slide confronta macchine virtuali e container. Una macchina virtuale include un sistema operativo guest completo e viene eseguita tramite un hypervisor. È ben isolata, ma richiede più CPU, RAM e tempo di avvio. Un container Docker, invece, contiene l'applicazione e le sue dipendenze e condivide il kernel del sistema operativo host. È quindi più leggero, si avvia rapidamente e rende più semplice spostare il progetto tra sviluppo, CI e produzione.

Per Split Mate scegliamo Docker perché frontend e backend devono essere eseguiti in ambienti coerenti. Se ogni sviluppatore installasse manualmente versioni diverse di .NET, Node o Nginx, potrebbero comparire differenze difficili da diagnosticare. L'immagine Docker descrive l'ambiente in modo riproducibile e riduce il problema del "funziona solo sul mio computer". Nelle slide successive vedremo come vengono costruite queste immagini e come i container vengono coordinati.
