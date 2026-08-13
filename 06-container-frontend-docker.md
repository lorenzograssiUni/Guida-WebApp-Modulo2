# 06 - Progettazione del container Docker del frontend

Questo capitolo descrive come è stato containerizzato il frontend React/Vite del progetto Split Mate, evidenziando gli aspetti di build, serving e configurazione dell'URL dell'API.[cite:30]

## 6.1 Struttura del progetto frontend

La cartella `frontend-gestione-spese` contiene l'applicazione React/Vite che implementa l'interfaccia utente per la gestione di gruppi, spese e riepiloghi.[cite:30]

Gli elementi principali sono:

- Codice React (componenti, pagine, router).
- Configurazione Vite per la build del bundle statico.
- Dipendenze gestite tramite `package.json` e `npm`.

Nel contesto della guida, il frontend è importante perché mostra come un'applicazione SPA possa essere distribuita sia come sito statico su Vercel sia come contenuto servito da un container Docker.

## 6.2 Dockerfile del frontend e build

Il `Dockerfile` del frontend (nella cartella `frontend-gestione-spese`) definisce i passaggi per:[cite:30]

- Installare le dipendenze Node.js necessarie alla build.
- Eseguire `npm run build` per generare il bundle statico (file HTML, CSS, JS ottimizzati).
- Copiare il risultato della build in un'immagine base (ad esempio nginx) che si occupa di servire i file statici.

Questo riflette il pattern comune per applicazioni SPA:

1. Fase di **build** in ambiente Node (tooling).
2. Fase di **serving** con un web server leggero ottimizzato per contenuti statici.

## 6.3 Configurazione di VITE_API_URL e integrazione con il backend

Nel `docker-compose.yml` è presente una sezione dedicata al frontend:[cite:30]

```yaml
  frontend:
    build:
      context: ./frontend-gestione-spese
      dockerfile: Dockerfile
      args:
        VITE_API_URL: http://localhost:5207/api
    container_name: splitmate-frontend
    ports:
      - "3000:80"
    depends_on:
      backend:
        condition: service_started
```

Il **build arg** `VITE_API_URL` è utilizzato da Vite per configurare, al momento della build, l'indirizzo dell'API backend da chiamare:

- In ambiente locale Docker: `http://localhost:5207/api` (il backend gira sulla macchina host ed espone la porta 5207).[cite:30]
- In deployment online: la variabile viene impostata al valore dell'URL Azure App Service, in modo che il frontend su Vercel chiami l'API corretta.[cite:30]

Questo meccanismo collega direttamente la teoria sulle applicazioni stateless e sulla separazione frontend/backend alla pratica della configurazione tramite variabili di build.

## 6.4 Orchestrazione del frontend nel compose

Il servizio frontend dipende dal backend tramite `depends_on` con `condition: service_started`, assicurando che il backend sia almeno avviato quando il frontend parte.[cite:30]

L'esposizione della porta `3000:80` permette agli utenti di accedere all'applicazione tramite `http://localhost:3000`, mentre il container ascolta sulla porta 80 interna.

Insieme al container backend, questo setup mostra un tipico scenario di **multi‑container** orchestrato, coerente con la teoria su deployment di applicazioni stateless su più istanze e sulla gestione del traffico tra client, frontend e API.
