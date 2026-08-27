# Traccia 1 — Introduzione e fondamenti di Split Mate

## Obiettivo dell'esposizione
Presentare il problema affrontato da Split Mate, l'idea generale dell'applicazione e i concetti fondamentali di cloud computing sui quali si basa il progetto.

## 1. Che cos'è·°Split Mate
Split Mate è un'applicazione web pensata per dividere e gestire le spese condivise. L'esigenza nasce in situazioni come viaggi, convivenze, cene o attività di gruppo: ogni partecipante può sostenere alcune spese e il sistema calcola quanto ciascuno deve pagare o ricevere.

L'applicazione separa il frontend, cioè l'interfaccia usata dall'utente, dal backend, che espone le API e contiene la logica applicativa. I dati vengono gestiti da un database. Questa divisione rende il progetto più ordinato e permette di distribuire i componenti su servizi differenti.

## 2. Perché usare il cloud
Il cloud computing permette di utilizzare risorse informatiche tramite Internet senza dover gestire direttamente tutto l'hardware. Le risorse possono essere calcolo, rete, archiviazione o servizi già configurati.

Nel caso di Split Mate il cloud è utile perché consente di pubblicare il backend e il frontend, renderli raggiungibili dagli utenti e adattare più facilmente l'infrastruttura alle necessità del progetto. Inoltre, servizi gestiti riducono il lavoro necessario per installazione, manutenzione e aggiornamento dei server.

## 3. Definizione NIST
La definizione NIST descrive il cloud attraverso alcune caratteristiche essenziali:

- self-service su richiesta: l'utente può ottenere risorse senza intervento manuale continuo del fornitore;
- accesso tramite rete: i servizi sono raggiungibili attraverso connessioni di rete e dispositivi diversi;
- condivisione delle risorse: l'infrastruttura viene utilizzata da più clienti in modo isolato;
- elasticità·°: le risorse possono aumentare o diminuire in base al carico;
- servizio misurato: l'utilizzo delle risorse può essere monitorato e contabilizzato.

Queste caratteristiche distinguono un vero servizio cloud da un semplice server remoto.

## 4. Modelli di servizio e deployment
I principali modelli di servizio sono IaaS, PaaS e SaaS. Con IaaS il provider offre macchine virtuali, rete e storage, lasciando al cliente una maggiore responsabilità. Con PaaS il provider gestisce anche parte dell'ambiente di esecuzione, mentre lo sviluppatore si concentra sul codice. Con SaaS l'utente utilizza direttamente un'applicazione completa.

Per Split Mate è particolarmente interessante il modello PaaS: Azure App Service può ospitare il backend senza richiedere la gestione manuale di una macchina virtuale, mentre Vercel può pubblicare il frontend.

## Collegamento al relatore successivo
Dopo aver chiarito lo scopo dell'applicazione e il significato del cloud, il secondo studente approfondirà·°virtualizzazione, container e immagini Docker, che permettono di rendere l'esecuzione più riproducibile.
