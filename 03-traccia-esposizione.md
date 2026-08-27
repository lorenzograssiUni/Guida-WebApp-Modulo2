# Traccia 3 — Docker Compose, CI/CD e qualità

## Obiettivo dell'esposizione
Descrivere come Split Mate viene eseguito localmente come sistema composto da più servizi e come la pipeline CI/CD verifica e prepara il software in modo automatico.

## 1. Perché serve l'orchestrazione
Split Mate non è formato da un solo processo. Frontend, backend e database hanno responsabilità diverse e devono comunicare tramite rete. Avviare ogni componente manualmente richiederebbe molti comandi e aumenterebbe il rischio di usare configurazioni incoerenti.

L'orchestrazione descrive i servizi, le reti, i volumi, le porte e le variabili necessarie per avviare l'ambiente. In questo progetto Docker Compose rappresenta il modo più semplice per coordinare lo stack durante lo sviluppo.

## 2. File Docker Compose
Nel file Compose ogni servizio indica almeno l'immagine o il contesto di build, le porte esposte e le configurazioni necessarie. I servizi possono comunicare usando i nomi definiti nel file invece di indirizzi IP scritti manualmente.

Un database deve normalmente usare un volume per conservare i dati anche se il container viene ricreato. Senza un volume, la cancellazione del container può causare la perdita dei dati memorizzati nel suo filesystem temporaneo.

La direttiva `depends_on` può esprimere l'ordine di avvio, ma non garantisce sempre che un servizio sia già pronto ad accettare richieste. Per questo il backend dovrebbe gestire correttamente i tentativi di connessione e, quando possibile, usare healthcheck.

## 3. Sviluppo e produzione
In sviluppo si privilegiano velocità e facilità·°di debug: si possono montare directory locali, usare log dettagliati e accedere direttamente alle porte. In produzione, invece, sono importanti immagini minimali, segreti esterni, logging controllato, aggiornamenti riproducibili e minori privilegi.

Questa distinzione evita di portare in produzione configurazioni comode ma insicure, come password nel repository o modalità di debug attive.

## 4. Pipeline CI/CD
CI significa Continuous Integration: a ogni modifica il codice viene costruito e verificato automaticamente. CD indica la consegna o il deployment continuo, cioè la possibilità di pubblicare automaticamente una versione dopo i controlli.

GitHub Actions definisce workflow eseguiti in risposta a eventi come push e pull request. Una pipeline tipica per Split Mate può:

- fare il checkout del repository;
- configurare l'ambiente JavaScript e Java, se entrambi sono presenti;
- installare le dipendenze;
- eseguire lint e test;
- costruire le immagini Docker;
- pubblicare le immagini in un registry;
- avviare il deployment dopo i controlli riusciti.

## 5. Test e segreti
I test automatici sono un controllo di qualità prima del deployment. I test unitari verificano funzioni o componenti isolati; i test di integrazione controllano la collaborazione tra componenti; i test end-to-end simulano flussi completi dell'utente.

Token, password e chiavi API non devono essere scritti nei file YAML o nel codice. GitHub Actions mette a disposizione Secrets e variabili d'ambiente, così la pipeline può usare credenziali senza renderle pubbliche.

## Collegamento al relatore successivo
Dopo la parte relativa a esecuzione locale, automazione e qualità, il quarto studente parlerà·°del deployment reale del backend e del frontend e degli aspetti di stato e scalabilità·°.
