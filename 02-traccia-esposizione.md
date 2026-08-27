# Traccia 2 — Docker nel progetto Split Mate

## Obiettivo
Mostrare come Docker è usato in Split Mate per backend e frontend, partendo dai Dockerfile presenti nel repository.

## 1. Dockerfile del backend
Il backend ha un Dockerfile che:
- parte da un'immagine base (es. Node/Java a seconda dell'implementazione);
- copia il codice del progetto;
- installa le dipendenze;
- definisce il comando di avvio del server.

L'immagine risultante contiene tutto il necessario per eseguire il backend. Le variabili d'ambiente (es. URL del database, porte) sono passate a runtime, non fissate nell'immagine.

Questo permette di eseguire lo stesso backend in locale, in CI e su Azure senza cambiare l'immagine.

## 2. Dockerfile del frontend
Il frontend ha un Dockerfile che:
- installa le dipendenze (es. npm/yarn);
- esegue la build (es. Vite);
- serve i file statici generati con un server web leggero.

Un punto importante è la configurazione dell'URL del backend: in locale può puntare a `localhost`, ma nell'immagine usata in produzione deve puntare all'URL pubblico su Azure.

## 3. Virtualizzazione vs container in Split Mate
Split Mate usa container, non macchine virtuali complete:
- i container condividono il kernel dell'host;
- sono più leggeri e si avviano rapidamente;
- ogni servizio (backend, frontend, database) gira nel proprio container.

La virtualizzazione classica (VM) sarebbe più pesante e meno adatta a questo tipo di architettura microservizi.

## 4. Immagini e riproducibilità·°
Usare immagini Docker garantisce che:
- l'ambiente di sviluppo locale sia simile a quello di CI e produzione;
- le dipendenze siano fissate nell'immagine;
- i problemi di "funziona sulla mia macchina" siano ridotti.

Nel progetto le immagini sono costruite sia in locale (con `docker build`) sia nella pipeline GitHub Actions.

## Collegamento al relatore successivo
Il terzo studente spiega come Docker Compose coordina i container in locale e come GitHub Actions automatizza build, test e creazione delle immagini.
