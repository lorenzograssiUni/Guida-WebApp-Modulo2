# Traccia 3 — Docker Compose e CI/CD in Split Mate

## Obiettivo
Descrivere come Split Mate è eseguito in locale con Docker Compose e come la pipeline CI/CD automatizza build, test e immagini.

## 1. Docker Compose nel progetto
Split Mate è composto da più servizi:
- backend;
- frontend;
- database.

Il file Docker Compose:
- definisce i servizi;
- specifica le immagini o i contesti di build;
- espone le porte necessarie;
- configura le variabili d'ambiente;
- definisce volumi per il database (per non perdere i dati).

I servizi comunicano usando i nomi definiti nel file Compose, non indirizzi IP fissi.

## 2. Sviluppo vs produzione
In sviluppo:
- si usano volumi montati dal filesystem locale per vedere subito le modifiche;
- i log sono più dettagliati;
- le porte sono esposte per accesso diretto.

In produzione:
- le immagini sono più minimali;
- i segreti (password, token) sono gestiti dal provider, non nel repository;
- il logging è controllato e strutturato.

Questa distinzione evita di portare in produzione configurazioni comode ma insicure.

## 3. Pipeline CI/CD con GitHub Actions
Nel repository c'è·°un workflow GitHub Actions che:
- parte su push o pull request;
- fa il checkout del codice;
- configura gli ambienti necessari (es. Node, Java);
- installa le dipendenze;
- esegue lint e test;
- costruisce le immagini Docker;
- può pubblicare le immagini su un registry;
- può avviare il deployment se i test passano.

Questo automatizza la verifica della qualità e la preparazione delle immagini per il deployment.

## 4. Test e segreti
I test automatici includono:
- test unitari (funzioni o componenti isolati);
- test di integrazione (interazione tra componenti);
- eventualmente test end-to-end (flussi completi).

I segreti (token, password, chiavi) non sono nel repository: sono definiti nei Secrets di GitHub Actions e usati come variabili d'ambiente nella pipeline.

## Collegamento al relatore successivo
Il quarto studente entra nel dettaglio del deployment reale: backend su Azure, frontend su Vercel, gestione dello stato e scalabilità·°.
