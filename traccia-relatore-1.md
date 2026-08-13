# Traccia Relatore 1

Relatore 1 presenta le **slide 1–3**: introduzione al progetto e teoria base sul cloud.

---

## Slide 1 – Split Mate – Gestore Spese di Gruppo

1. **Apertura**
   - "In questa presentazione vi parliamo di *Split Mate – Gestore Spese di Gruppo*, il progetto del modulo 2 WebApp."
   - "È una web application pensata per gestire e dividere le spese tra coinquilini, gruppi di amici, viaggi, cene, ecc."[cite:30]

2. **Descrizione tecnica sintetica**
   - "L'app è full‑stack: frontend React/Vite, backend ASP.NET Core, database SQLite."[cite:30][cite:32]
   - "Questo ci permette di toccare sia gli aspetti di sviluppo che quelli di deployment."[cite:32]

3. **Ambienti di esecuzione**
   - "La stessa applicazione gira in due contesti: in locale tramite Docker Compose e online come servizi cloud (backend su Azure App Service, frontend su Vercel)."[cite:30]

4. **Obiettivo dell’esposizione**
   - "Il focus di oggi non è il codice in dettaglio, ma come abbiamo applicato la teoria del modulo 2: cloud computing, virtualizzazione, container Docker e pipeline CI/CD."[cite:29]

---

## Slide 2 – Cloud computing secondo NIST

1. **Definizione NIST (in parole tue)**
   - "Secondo il NIST, il cloud computing è un modello che permette di accedere, via rete, a risorse di calcolo condivise – server, storage, applicazioni – in modo rapido e con pochissimo intervento manuale del fornitore."[cite:29]

2. **Cinque caratteristiche essenziali**
   - Elenca e spiega brevemente:
     - "On‑demand self‑service: l’utente può creare e gestire risorse da solo, tramite console."[cite:29]
     - "Ampio accesso alla rete: posso raggiungere i servizi da ovunque, con vari dispositivi."[cite:29]
     - "Pooling di risorse: l’infrastruttura è condivisa tra più clienti in modalità multi‑tenant."[cite:29]
     - "Elasticità rapida: le risorse si adattano automaticamente al carico, aumentando o diminuendo."[cite:29]
     - "Servizio misurato: pagamento basato sull’uso effettivo, modello pay‑per‑use."[cite:29]

3. **Collegamento al progetto**
   - "Per noi queste proprietà sono importanti perché il progetto è pensato per girare su servizi cloud che si gestiscono in modo automatico, come App Service e Vercel."[cite:30]

---

## Slide 3 – Modelli di servizio e deployment + aggancio a Split Mate

1. **Modelli di servizio (IaaS, PaaS, SaaS)**
   - "IaaS: affitto infrastruttura (VM, rete, storage), ma gestisco io sistemi operativi e applicazioni."[cite:29]
   - "PaaS: il provider gestisce anche l’OS e il runtime, io penso solo al mio codice."[cite:29]
   - "SaaS: uso direttamente un’applicazione finita via Internet, senza occuparmi di infrastruttura."[cite:29]

2. **Modelli di deployment**
   - "Cloud pubblico: servizi messi a disposizione di chiunque via Internet."[cite:29]
   - "Cloud privato: infrastruttura dedicata a una sola organizzazione."[cite:29]
   - "Cloud ibrido: combinazione di pubblico e privato."[cite:29]
   - "Multi‑cloud: uso contemporaneo di più provider, ognuno per un servizio specifico."[cite:29]

3. **Come si inserisce Split Mate**
   - "Nel nostro progetto, il backend è pubblicato su Azure App Service, che è un servizio PaaS: Azure gestisce VM, OS e runtime .NET."[cite:29][cite:44]
   - "Il frontend React è distribuìto su Vercel, piattaforma specializzata per applicazioni web stateless, assimilabile a un servizio SaaS/PaaS."[cite:45]
   - "Insieme, queste scelte realizzano uno scenario di *cloud pubblico multi‑cloud*: sfruttiamo provider diversi (Microsoft Azure per il backend, Vercel per il frontend) integrati via HTTP."[cite:29][cite:30]

4. **Passaggio al relatore 2**
   - "Fin qui abbiamo visto la teoria base del cloud. Adesso passiamo alla virtualizzazione e ai container, per capire perché abbiamo scelto Docker e come si inserisce nell’architettura del progetto."
