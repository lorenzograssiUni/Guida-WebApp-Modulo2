# 02 - Fondamenti di Cloud Computing secondo NIST

Questo capitolo riassume i concetti teorici di cloud computing utilizzati come base per interpretare le scelte di deployment del progetto Split Mate.[cite:29]

## 2.1 Definizione NIST e caratteristiche essenziali

Il NIST definisce il cloud computing come un modello che permette, da qualsiasi luogo ed in maniera comoda, l'accesso su richiesta tramite rete ad un insieme di risorse di elaborazione condivise e configurabili (reti, server, storage, applicazioni e servizi), rapidamente fornite e rilasciate con il minimo sforzo di gestione o interazione da parte del fornitore.[cite:29]

Le cinque caratteristiche essenziali del cloud sono:[cite:29]

- **On‑demand self‑service**: gli utenti possono autonomamente creare e gestire risorse cloud tramite console web, senza intervento manuale.
- **Ampio accesso alla rete**: le risorse cloud sono accessibili ovunque tramite Internet e dispositivi diversi.
- **Pooling di risorse**: le risorse cloud sono condivise tra utenti in ambiente multi‑tenant.
- **Elasticità rapida**: le risorse possono aumentare o diminuire automaticamente in base alle esigenze.
- **Servizio misurato**: il consumo delle risorse è monitorato e fatturato in base all’uso reale.

Queste proprietà sono richiamate nel progetto quando si discute di scalabilità, elasticità e modelli di pagamento pay‑per‑use.

## 2.2 Benefici e criticità del cloud

Il materiale del corso evidenzia diversi **benefici** del cloud computing:[cite:29]

- Maggiore agilità: attivare risorse in minuti anziché settimane.
- Riduzione dei costi: eliminazione dei costi iniziali elevati e manutenzione demandata al provider.
- Modello a consumo: si paga solo ciò che si usa.
- Resilienza e ridondanza globale: data center distribuiti garantiscono alta disponibilità.

Sono anche elencate le **criticità** principali:[cite:29]

- Vendor lock‑in: migrazione complessa tra provider.
- Sicurezza dei dati e governance.
- Necessità di competenze FinOps e gestione oculata dei costi.
- Nuove skill DevOps e cambio di mentalità.
- Dipendenza dalla connettività di rete.

Nel manuale queste considerazioni saranno collegate al fatto che Split Mate utilizza servizi gestiti (App Service, Vercel) e che il progetto potrebbe in futuro spostarsi tra provider diversi.

## 2.3 Modelli di servizio: IaaS, PaaS, SaaS

Il NIST distingue tre principali modelli di servizio "as a service":[cite:29]

- **Infrastructure as a Service (IaaS)**: il provider gestisce l'infrastruttura di base (server, rete, virtualizzazione), mentre l'utente mantiene il controllo su sistema operativo, applicazioni e dati.
- **Platform as a Service (PaaS)**: il provider gestisce l'hardware e il sistema operativo, consentendo all'utente di concentrarsi sullo sviluppo e la gestione delle applicazioni.
- **Software as a Service (SaaS)**: l'utente accede ad un'applicazione completa e funzionante tramite Internet, senza occuparsi della gestione infrastrutturale.

Nel progetto Split Mate, il backend .NET pubblicato su Azure App Service è un esempio di **PaaS**, mentre il frontend React ospitato su Vercel è assimilabile ad un servizio SaaS/PaaS per applicazioni web stateless.[cite:30]

## 2.4 Modelli di deployment

Il NIST definisce quattro modelli principali di deployment:[cite:29]

- **Cloud pubblico**: servizi forniti da un provider di terze parti e disponibili al pubblico tramite Internet.
- **Cloud privato**: infrastruttura dedicata ad una singola organizzazione.
- **Cloud ibrido**: combinazione di cloud pubblico e privato.
- **Multi‑cloud**: uso contemporaneo di più fornitori diversi.

È inoltre menzionato il **community cloud**, riservato ad un gruppo specifico di organizzazioni con obiettivi comuni.[cite:29]

Il progetto Split Mate implementa uno scenario **multi‑cloud pubblico**: il backend è pubblicato su Azure (cloud pubblico Microsoft) e il frontend su Vercel (piattaforma di deploy web), illustrando come un'applicazione possa usare servizi di fornitori diversi mantenendo coerenza applicativa.[cite:30]
