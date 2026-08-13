# 05 - Progettazione del container Docker del backend

Questo capitolo analizza in dettaglio il container Docker del backend ASP.NET Core, mostrando come le scelte di configurazione si collegano alla teoria su virtualizzazione, gestione dello stato e deployment.[cite:29][cite:32]

## 5.1 Dockerfile del backend ASP.NET Core

Il file `gestione-spese/Dockerfile` definisce come viene costruita l'immagine Docker del backend.[cite:32]

Gli elementi chiave sono:

- **Immagine base**: una immagine ufficiale .NET (SDK per la fase di build, runtime per la fase di esecuzione), che incapsula il runtime necessario per avviare l'app.
- **Copia del progetto**: il sorgente della web application viene copiato nella directory `/app` dell'immagine.
- **Restore e build**: vengono eseguiti `dotnet restore` e `dotnet build/publish` all'interno del container, sfruttando il file `NuGet.Config` per controllare le fonti dei pacchetti.[cite:32]
- **Entrypoint**: il container viene avviato eseguendo `dotnet gestione-spese.dll`, ascoltando sulla porta configurata (`5207`).

Questa struttura segue il pattern "build stage + runtime stage" tipico delle applicazioni .NET containerizzate, riducendo la dimensione finale dell'immagine e separando la fase di compilazione dalla fase di esecuzione.

## 5.2 Configurazioni appsettings e ambienti

Nella cartella `gestione-spese` sono presenti diversi file di configurazione:[cite:32]

- `appsettings.json`: configurazione di base, includendo la connection string di default.
- `appsettings.Development.json`: override per l'ambiente di sviluppo locale.
- `appsettings.Docker.json`: configurazione specifica per quando il backend gira in ambiente Docker.

Il `Program.cs` carica le configurazioni in base all'ambiente (`builder.Environment.EnvironmentName`), permettendo di avere:[cite:32]

- Path del database diverso tra Docker e deploy online.
- URL di ascolto e altre opzioni adattate al contesto.

Questa separazione riflette i concetti teorici di gestione degli ambienti (Development, Production, Docker) e permette di allineare la configurazione del container alle esigenze di persistenza e networking.

## 5.3 Gestione del database SQLite nei container

Nel `Program.cs` è presente una logica esplicita per scegliere il path del database in base all'ambiente:[cite:32]

- Se l'ambiente è `Docker`, il path del DB è `/app/data/gestionespese.db`.
- Negli altri casi (es. deployment su Azure), il path è `C:\\home\\gestionespese.db`.

Il file `docker-compose.yml` monta un **volume** `sqlite-data` sulla directory `/app/data` del container backend:[cite:30]

```yaml
services:
  backend:
    ...
    volumes:
      - sqlite-data:/app/data
```

In questo modo:

- Il file SQLite è persistente anche se il container viene ricreato.
- Lo storage dei dati è separato dall'immagine applicativa, in linea con i principi di gestione dello stato (stateless con stato esterno) discussi a livello teorico.[cite:29]

## 5.4 Environment variables e URL di ascolto

Nel servizio backend di `docker-compose.yml` sono definite alcune variabili di ambiente fondamentali:[cite:30]

- `DOTNET_ENVIRONMENT=Docker` e `ASPNETCORE_ENVIRONMENT=Docker`: assicurano che l'applicazione carichi le configurazioni corrette e che il ramo Docker di `Program.cs` venga utilizzato.
- `ASPNETCORE_URLS=http://+:5207`: indica al runtime .NET di ascoltare sulla porta 5207 su tutte le interfacce.

Queste variabili collegano direttamente la teoria sugli **ambienti di runtime** e sulla **configurazione via environment** alla pratica del container: la stessa immagine può comportarsi in modo diverso a seconda dell'ambiente, senza modificare il codice.
