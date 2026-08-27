# Traccia 1 — Split Mate: scopo, architettura e cloud

## Obiettivo
Presentare Split Mate, il problema che risolve, l'architettura generale e come il cloud è usato nel progetto.

## 1. Che cos'è·°Split Mate
Split Mate è un'applicazione web per dividere e gestire spese condivise (viaggi, convivenze, cene, attività di gruppo). Ogni utente può registrare spese e il sistema calcola quanto ciascuno deve pagare o ricevere.

L'applicazione è divisa in:
- frontend (interfaccia utente);
- backend (API e logica di gestione spese);
- database (persistenza dei dati).

Questa separazione permette di distribuire i componenti su servizi diversi.

## 2. Architettura del progetto
Il backend espone API REST per creare gruppi, aggiungere spese, assegnare partecipanti e calcolare i saldi. Il frontend chiama queste API e mostra i risultati all'utente.

Nel repository ci sono:
- Dockerfile per il backend;
- Dockerfile per il frontend;
- file Docker Compose per orchestrare i servizi in locale;
- workflow GitHub Actions per CI/CD;
- configurazioni per il deployment su Azure (backend) e Vercel (frontend).

## 3. Perché il cloud in Split Mate
Il cloud è usato per:
- pubblicare il backend su Azure App Service;
- pubblicare il frontend su Vercel;
- rendere l'applicazione raggiungibile da qualsiasi dispositivo;
- semplificare la gestione dell'infrastruttura (PaaS).

In Split Mate si usa soprattutto il modello PaaS: Azure App Service gestisce il runtime del backend, Vercel gestisce la distribuzione del frontend.

## 4. Richiami teorici essenziali (NIST)
La definizione NIST descrive il cloud con:
- self-service su richiesta;
- accesso tramite rete;
- condivisione delle risorse;
- elasticità·°;
- servizio misurato.

Nel progetto queste caratteristiche si vedono nel fatto che:
- si attivano servizi (App Service, Vercel) senza gestire macchine fisiche;
- l'app è accessibile via browser;
- le risorse sono condivise con altri clienti del provider;
- si può scalare cambiando piano o istanze.

## Collegamento al relatore successivo
Il secondo studente entra nel dettaglio di come backend e frontend sono containerizzati con Docker e come i Dockerfile sono scritti nel progetto.
