# Traccia 4 — Deployment, stato e scalabilità·°

## Obiettivo dell'esposizione
Spiegare come i componenti di Split Mate possono essere pubblicati online e quali problemi bisogna considerare quando l'applicazione passa dall'ambiente locale a un ambiente cloud.

## 1. Deployment del backend su Azure App Service
Azure App Service è una piattaforma gestita per ospitare applicazioni web e API. Il provider si occupa di molte attività infrastrutturali, mentre il team configura l'applicazione, il piano di esecuzione, le variabili e il metodo di deployment.

Il backend può essere distribuito a partire dal codice oppure da un'immagine Docker. Nel secondo caso il processo consiste nel costruire l'immagine, pubblicarla in un container registry e configurare App Service affinché la utilizzi.

La configurazione deve includere almeno porta di ascolto, ambiente di esecuzione, URL del database e impostazioni CORS. Le credenziali non devono essere versionate: vanno conservate nelle impostazioni sicure del servizio.

## 2. Deployment del frontend su Vercel
Vercel è adatto alla pubblicazione di frontend moderni e applicazioni generate con strumenti come Vite. Il collegamento con il repository permette di avviare automaticamente una nuova build quando vengono pubblicate modifiche.

Durante la build vengono installate le dipendenze ed eseguito il comando previsto dal progetto. Il risultato viene poi servito tramite la rete globale del provider.

Una variabile importante è l'URL pubblico del backend. Il frontend deve chiamare l'API ospitata su Azure e non un indirizzo locale. Se frontend e backend appartengono a origini diverse, il backend deve autorizzare l'origine del frontend tramite CORS.

## 3. Stato e sessioni
Le richieste HTTP sono normalmente stateless: ogni richiesta dovrebbe contenere le informazioni necessarie per essere elaborata. Questo facilita la distribuzione delle richieste tra più istanze.

Se l'applicazione usa sessioni memorizzate nella memoria di una singola istanza, una richiesta successiva instradata verso un'altra istanza potrebbe non trovare i dati. Le alternative sono usare token stateless, per esempio JWT con adeguate protezioni, oppure un archivio condiviso come Redis o un database.

Anche i file caricati e i dati temporanei non dovrebbero dipendere dal disco locale del container: gli ambienti cloud possono riavviare o sostituire l'istanza. Per i dati persistenti servono database, object storage o volumi gestiti.

## 4. Scalabilità·°e affidabilità·°
La scalabilità·°orizzontale consiste nell'aggiungere istanze dell'applicazione; quella verticale nell'aumentare le risorse di un'istanza. La prima è spesso più adatta alle architetture cloud, ma richiede che il backend sia progettato senza stato locale non condiviso.

Per migliorare l'affidabilità·°sono utili healthcheck, log, metriche, timeout, retry controllati e backup del database. Bisogna distinguere tra disponibilità del servizio e correttezza dei dati: rendere il backend replicabile non sostituisce una corretta strategia di persistenza.

## 5. Sintesi finale
L'architettura di Split Mate combina separazione dei componenti, containerizzazione, orchestrazione locale e servizi cloud gestiti. Docker rende riproducibile l'ambiente; Compose coordina i servizi durante lo sviluppo; GitHub Actions automatizza i controlli; Azure ospita il backend e Vercel il frontend.

Il risultato è un flusso completo: sviluppo locale, verifica automatica, costruzione delle immagini e pubblicazione. Le decisioni più importanti riguardano sicurezza dei segreti, configurazione delle origini, persistenza dei dati e gestione dello stato.

## Chiusura dell'esposizione
Si può concludere sottolineando che il cloud non significa soltanto "mettere online" un'applicazione. Significa progettare componenti distribuibili, automatizzare il ciclo di rilascio e scegliere dove collocare dati e responsabilità operative. Split Mate mostra questo percorso attraverso un caso concreto e facilmente collegabile ai concetti teorici.