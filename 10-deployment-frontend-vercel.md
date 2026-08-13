# 10 - Deployment del frontend su Vercel

Questo capitolo descrive come il frontend React/Vite di Split Mate viene distribuito su **Vercel**, collegando la pratica del deploy di una Single Page Application ai concetti teorici di cloud pubblico e applicazioni stateless.[cite:29][cite:30]

## 10.1 Vercel come piattaforma per web stateless

Vercel è una piattaforma cloud pensata per distribuire applicazioni web front‑end e siti statici:

- Si occupa automaticamente di build, hosting, CDN e routing.
- Offre integrazione diretta con GitHub per il deploy continuo.

Dal punto di vista dei modelli NIST, Vercel può essere vista come un servizio SaaS/PaaS per applicazioni web stateless: lo sviluppatore non gestisce server o sistemi operativi, ma solo il codice del frontend.[cite:29]

Nel progetto Split Mate, il frontend viene pubblicato su un URL Vercel indicato nel README.[cite:30]

## 10.2 Flusso di build e deploy

Il flusso tipico di deploy del frontend su Vercel è:[cite:30]

1. Collegare la repository GitHub `gestore-spese` a Vercel.
2. Configurare il comando di build (ad esempio `npm run build`) e la directory di output generata da Vite.
3. Impostare la variabile di ambiente `VITE_API_URL` per puntare all'URL dell'API Azure.
4. Ogni push sul branch configurato (ad esempio `main`) innesca una nuova build e un nuovo deploy.

Questo flusso riflette i principi di CI/CD applicati al frontend: ogni modifica al codice viene automaticamente compilata e resa disponibile online.

## 10.3 Collegamento tra frontend Vercel e backend Azure

Il frontend React/Vite, una volta deployato su Vercel, comunica con il backend pubblicato su Azure App Service tramite l'URL configurato in `VITE_API_URL`:[cite:30][cite:41]

- Le chiamate HTTP del frontend sono indirizzate all'endpoint pubblico dell'API.
- L'applicazione rimane stateless lato frontend: non memorizza stato persistente, ma si affida alle API per leggere e scrivere dati.

Questo pattern illustra la separazione di responsabilità tra:

- **Layer di presentazione** (frontend su Vercel).
- **Layer di business e dati** (backend e database su Azure).

## 10.4 Benefici e limiti rispetto alla teoria cloud

Rispetto ai concetti teorici di cloud computing:[cite:29]

- L'uso di Vercel porta benefici di **scalabilità automatica**, **distribuzione geografica** tramite CDN e **time‑to‑market ridotto**.
- La natura stateless dell'app front‑end è coerente con i principi di scalabilità orizzontale e autoscaling: più istanze del frontend possono servire richieste senza problemi di stato condiviso.

I limiti principali su cui riflettere in ottica didattica sono:

- La dipendenza da un provider specifico (potenziale vendor lock‑in).
- La necessità di configurare correttamente le variabili di ambiente per mantenere la coerenza tra ambienti locale e cloud.

Nel complesso, il deployment su Vercel rende il progetto Split Mate un esempio concreto di applicazione multi‑cloud che sfrutta servizi specializzati per frontend e backend.
