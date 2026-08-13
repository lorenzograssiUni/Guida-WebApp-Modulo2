# 03 - Virtualizzazione e basi per i container

Questo capitolo approfondisce la virtualizzazione, le macchine virtuali e il loro rapporto con i container, collegando direttamente la teoria del modulo 2 alla scelta di usare Docker per lo sviluppo locale di Split Mate.[cite:29][cite:32]

## 3.1 Virtualizzazione hardware e hypervisor

La virtualizzazione è la tecnologia che rende possibile il cloud computing, permettendo di creare versioni virtuali di risorse fisiche (CPU, memoria, storage, rete) su una singola macchina fisica.[cite:29]

L'**hypervisor** (Virtual Machine Monitor) è il software che gestisce le macchine virtuali, mediando l'accesso all'hardware fisico. Si occupa di distribuire le risorse hardware tra le diverse VM, creando un livello di astrazione che nasconde i dettagli del server sottostante.[cite:29]

## 3.2 Macchine virtuali (VM): vantaggi e svantaggi

Nel materiale del corso sono evidenziati i principali **vantaggi** delle VM:[cite:29]

- Forte isolamento tra le VM.
- Possibilità di utilizzare un sistema operativo guest completo.
- Elevata portabilità e ambienti di testing sicuri.

Gli **svantaggi** includono:[cite:29]

- Sovraccarico dovuto al sistema operativo guest.
- Maggiore consumo di risorse rispetto ai container.
- Tempi di avvio più lunghi e maggiore complessità di gestione.

Questi limiti sono alla base della diffusione dei **container**, che puntano a ridurre overhead e tempi di avvio.

## 3.3 Container: concetto base e differenze rispetto alle VM

I container (come quelli di Docker) condividono il kernel del sistema operativo host e isolano solo il processo e le dipendenze applicative. Rispetto alle VM:[cite:29]

- Non richiedono un sistema operativo guest completo.
- Si avviano molto più velocemente.
- Consumano meno risorse, permettendo maggiore densità di istanze.

Nel progetto Split Mate, il backend ASP.NET Core e il frontend React/Vite vengono eseguiti in container Docker separati, orchestrati da Docker Compose, proprio per sfruttare la leggerezza e la portabilità di questo modello.[cite:30][cite:32]

## 3.4 Snapshot, istanze virtuali nel cloud e autoscaling

Il materiale teorico introduce anche gli **snapshot** delle VM (fotografie dello stato in un preciso istante) e le **istanze virtuali nel cloud** nel modello IaaS, con caratteristiche come provisioning rapido, pay‑per‑use, elasticità e personalizzazione.[cite:29]

Si discutono inoltre:

- **Scalabilità verticale** (scale up): potenziare una macchina aggiungendo risorse.
- **Scalabilità orizzontale** (scale out): aggiungere nuove istanze in parallelo.
- **Autoscaling**: capacità di aumentare o diminuire risorse automaticamente in base alla domanda.[cite:29]

Questi concetti saranno richiamati nei capitoli sul deployment e sulle pipeline, per spiegare come il progetto potrebbe evolvere da un singolo container/backend verso scenari con più istanze e meccanismi di autoscaling gestiti dal provider cloud.
