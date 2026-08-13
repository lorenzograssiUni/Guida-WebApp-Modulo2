# 09 - Deployment del backend su Azure App Service

Questo capitolo descrive come il backend ASP.NET Core di Split Mate viene pubblicato su **Azure App Service**, collegando la pratica del deploy alla teoria sui modelli di servizio e deployment cloud.[cite:29][cite:30]

## 9.1 Azure App Service come PaaS

Azure App Service è un servizio **Platform as a Service (PaaS)**: il provider gestisce l'infrastruttura fisica, il sistema operativo, il runtime .NET e parte del networking, mentre lo sviluppatore si concentra sul codice e sulla configurazione dell'applicazione.[cite:29]

Nel contesto di Split Mate:

- Il progetto `gestione-spese` viene pubblicato su un App Service Windows.[cite:30][cite:32]
- L'applicazione è esposta tramite un URL pubblico (utilizzato dal frontend e dal workflow CI per chiamare le API).[cite:30][cite:41]

Questo modello riduce la complessità gestionale rispetto ad un deployment IaaS, in linea con la teoria su PaaS.

## 9.2 Processo di pubblicazione del backend

Il deploy del backend su Azure segue tipicamente questi passi:[cite:30][cite:32]

1. **Build/publish** del progetto .NET in configurazione Release (localmente o tramite pipeline).
2. **Creazione della risorsa App Service** sul portale Azure (piano di servizio + app web).
3. **Configurazione degli App Settings** (variabili di ambiente) per l'applicazione, ad esempio:
   - `ASPNETCORE_ENVIRONMENT=Production`.
   - Connection string verso il database (in questo progetto, SQLite su disco locale della VM).
4. **Distribuzione del pacchetto** pubblicato sull'App Service (via Visual Studio, Azure CLI o GitHub Actions).

Nel progetto didattico, il focus è sull'esposizione dell'URL dell'API e sulla corretta configurazione del path del database.

## 9.3 Gestione della persistenza del database su Azure

In ambiente App Service, il backend usa il path `C:\\home\\gestionespese.db` per il database SQLite.[cite:32]

La cartella `C:\\home` è la directory persistente di App Service:

- I file salvati in questa cartella sopravvivono ai redeploy e ai riavvii del sito.
- Questo consente di mantenere la persistenza dei dati tra aggiornamenti dell'applicazione.

La logica in `Program.cs` distingue tra ambiente Docker (DB su `/app/data`) e ambiente App Service (DB su `C:\\home`), collegando direttamente le configurazioni di deployment alla teoria su **stateless con persistenza esterna**.[cite:29][cite:32]

## 9.4 Scalabilità e autoscaling in App Service

Dal punto di vista teorico, Azure App Service supporta:

- **Scalabilità verticale**: modificando il piano di servizio (più CPU/RAM).[cite:29]
- **Scalabilità orizzontale**: aggiungendo più istanze dell'app con bilanciamento automatico.
- **Autoscaling**: regole che aumentano o diminuiscono il numero di istanze in base al carico.

Nel progetto Split Mate, il backend è inizialmente eseguito su una singola istanza, ma la struttura stateless dell'API e la persistenza su disco consentono, in prospettiva, di scalare orizzontalmente aggiungendo istanze e adottando tecniche come database esterni o session management distribuito.[cite:29]
