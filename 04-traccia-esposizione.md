# Traccia 4 — Deployment, stato e scalabilità·°in Split Mate

## Obiettivo
Spiegare come Split Mate è pubblicato online e quali aspetti pratici di stato e scalabilità·°sono stati considerati.

## 1. Backend su Azure App Service
Il backend è ospitato su Azure App Service:
- il provider gestisce l'infrastruttura sottostante;
- il team configura l'applicazione, il piano e le variabili;
- il deployment può avvenire da codice o da immagine Docker.

Nel caso basato su Docker:
- l'immagine è costruita nel repository;
- è pubblicata su un container registry;
- Azure App Service è configurato per usare quell'immagine.

La configurazione include:
- porta di ascolto;
- ambiente di esecuzione;
- URL del database;
- impostazioni CORS per accettare richieste dal frontend su Vercel.

Le credenziali del database e altri segreti sono nelle impostazioni sicure di Azure, non nel repository.

## 2. Frontend su Vercel
Il frontend è pubblicato su Vercel:
- il repository è collegato a Vercel;
- a ogni push parte una nuova build;
- i file statici generati sono serviti dalla rete globale di Vercel.

Una variabile fondamentale è l'URL del backend: il frontend deve chiamare l'API su Azure, non un indirizzo locale. Se le origini sono diverse, il backend deve abilitare CORS per l'origine del frontend.

## 3. Stato e sessioni in Split Mate
Le richieste HTTP sono stateless: ogni richiesta dovrebbe contenere le informazioni necessarie. Questo aiuta a distribuire le richieste tra più istanze del backend.

Se Split Mate usasse sessioni in memoria su una singola istanza, una richiesta successiva potrebbe finire su un'altra istanza e perdere lo stato. Le alternative sono:
- token stateless (es. JWT) con adeguate protezioni;
- archivio condiviso (Redis o database) per lo stato delle sessioni.

I file caricati e i dati temporanei non devono dipendere dal disco locale del container, perché l'istanza può essere riavviata o sostituita. Per i dati persistenti si usano database o object storage.

## 4. Scalabilità·°e affidabilità·°pratica
Scalabilità·°orizzontale: aggiungere più istanze del backend. Scalabilità·°verticale: aumentare le risorse di un'istanza. Per Split Mate, la scalabilità·°orizzontale è più naturale in cloud, ma richiede che il backend non dipenda da stato locale non condiviso.

Per migliorare l'affidabilità·°si possono usare:
- healthcheck per verificare che il servizio sia vivo;
- log e metriche per monitorare il comportamento;
- timeout e retry controllati nelle chiamate tra servizi;
- backup regolari del database.

## 5. Sintesi del progetto
Split Mate combina:
- separazione frontend/backend/database;
- containerizzazione con Docker;
- orchestrazione locale con Docker Compose;
- automazione CI/CD con GitHub Actions;
- deployment su Azure (backend) e Vercel (frontend).

Il risultato è un flusso completo: sviluppo locale, verifica automatica, costruzione delle immagini e pubblicazione. Le scelte principali riguardano sicurezza dei segreti, configurazione delle origini, persistenza dei dati e gestione dello stato.

## Chiusura
Il cloud in Split Mate non è solo "mettere online" un'applicazione. Significa progettare componenti distribuibili, automatizzare il ciclo di rilascio e scegliere dove collocare dati e responsabilità operative. Split Mate mostra questo percorso attraverso un caso concreto.