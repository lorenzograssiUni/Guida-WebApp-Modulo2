# Traccia 2 — Virtualizzazione, container e Docker

## Obiettivo dell'esposizione
Spiegare come virtualizzazione e container aiutano Split Mate a funzionare in modo coerente nei diversi ambienti, collegando la teoria alla configurazione Docker del progetto.

## 1. Virtualizzazione
La virtualizzazione permette di creare una rappresentazione software di una risorsa fisica, per esempio una macchina virtuale con sistema operativo, CPU, memoria e disco virtuali. Un hypervisor coordina l'uso dell'hardware e consente a più macchine virtuali di convivere sullo stesso host.

Il vantaggio principale è l'isolamento: un ambiente virtuale può avere dipendenze e configurazioni diverse dagli altri. Lo svantaggio è che ogni macchina virtuale include un sistema operativo completo, quindi richiede più memoria e spazio.

## 2. Containerizzazione
Un container isola un processo e le sue dipendenze, ma condivide il kernel del sistema operativo host. Per questo motivo è generalmente più leggero e avviabile più rapidamente rispetto a una macchina virtuale.

Un container non è una macchina virtuale completa: contiene ciò che serve all'applicazione per essere eseguita, mentre il sistema operativo di base viene condiviso. L'isolamento non è assoluto come quello di una VM e la sicurezza deve essere configurata correttamente.

## 3. Immagini e container Docker
Docker usa immagini immutabili come base per creare container. Un'immagine comprende il codice, le dipendenze e le istruzioni necessarie all'avvio. Il Dockerfile descrive come costruire quell'immagine attraverso una sequenza di livelli.

Il flusso tipico è:

1. si scrive il Dockerfile;
2. si costruisce l'immagine con `docker build`;
3. si avvia un container con `docker run`;
4. si pubblicano le porte necessarie per raggiungere il servizio;
5. si usano variabili d'ambiente per separare configurazione e codice.

Usare la stessa immagine in locale e nella pipeline riduce il rischio che l'applicazione funzioni sul computer dello sviluppatore ma non in produzione.

## 4. Docker per il backend
Il backend di Split Mate viene impacchettato in un'immagine Docker. Il progetto e le sue dipendenze vengono copiati nell'immagine e il comando di avvio esegue il server applicativo.

È· importante non inserire nell'immagine segreti, password o file di configurazione sensibili. I valori variabili, come gli URL del database, devono essere passati durante l'esecuzione tramite variabili d'ambiente o meccanismi sicuri del provider.

## 5. Docker per il frontend
Anche il frontend può essere gestito con Docker. In fase di build vengono installate le dipendenze e generati i file statici. Un server web può poi distribuire tali file.

Per il frontend è fondamentale configurare correttamente l'indirizzo del backend: un valore valido in locale, come `localhost`, non indica automaticamente il backend quando l'interfaccia è pubblicata online.

## Collegamento al relatore successivo
Dopo aver spiegato i singoli container, il terzo studente mostrerà·°come coordinarli con Docker Compose e come automatizzare build e test usando GitHub Actions.
