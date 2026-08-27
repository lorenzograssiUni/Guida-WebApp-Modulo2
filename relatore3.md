# Relatore 3 — Slide 9–11

## Testo da studiare

Dopo aver preparato i container e il flusso Git, vediamo come Split Mate verifica automaticamente il codice. Nel repository sono presenti workflow GitHub Actions, tra cui `.github/workflows/ci.yml`. Il workflow viene attivato da un push o da una Pull Request sul branch `main`.

Il primo passaggio è il checkout: il runner di GitHub Actions scarica il contenuto del repository nell'ambiente temporaneo in cui verranno eseguiti i job. Subito dopo vengono configurati gli ambienti necessari, in particolare .NET 10 per il backend ASP.NET Core e Node.js 20 per il frontend React.

Questa configurazione è importante perché la pipeline deve usare versioni note e coerenti con quelle dichiarate dal progetto. Se in locale si usa una versione diversa dal runner, un build può funzionare sul computer dello sviluppatore ma fallire in CI.

Il secondo passaggio è il restore pulito delle dipendenze. Per il backend viene usato il file `NuGet.Config` locale e vengono ripristinati i pacchetti NuGet. Per il frontend viene usato `npm ci`, che installa le dipendenze a partire dal lockfile in modo riproducibile. Rispetto a un'installazione generica, `npm ci` è pensato per ambienti automatizzati e mantiene esattamente le versioni definite dal progetto.

Il terzo passaggio è la build in modalità Release. Il comando `dotnet build` compila il backend ASP.NET Core; parallelamente il frontend viene costruito tramite Vite. Durante la build del frontend viene iniettata la variabile `VITE_API_URL`.

Questa variabile è essenziale perché il frontend deve conoscere l'indirizzo delle API. In locale può indicare il backend esposto sulla porta 5207, mentre nell'ambiente pubblicato deve indicare l'URL di Azure App Service. Il valore viene deciso dalla configurazione dell'ambiente e non deve essere scritto rigidamente nel codice.

La pipeline è un gate di qualità: se fallisce il restore, la compilazione del backend o la build del frontend, la modifica non dovrebbe essere integrata. In questo modo il branch principale rimane compilabile e il problema viene individuato subito, prima del deployment.

La slide successiva aggiunge la validazione Docker tramite `.github/workflows/docker.yml`. Qui non ci limitiamo a compilare il codice: costruiamo anche le immagini di test `splitmate-backend:ci` e `splitmate-frontend:ci`. Questo verifica che i Dockerfile siano corretti e che l'applicazione possa essere impacchettata nell'ambiente previsto.

Dopo la costruzione delle immagini viene eseguito `docker compose config --quiet`. Il comando valida la configurazione Docker Compose senza avviare necessariamente tutto lo stack. Controlla quindi che la sintassi e la struttura del file siano corrette e che il deployment non parta da una configurazione non valida.

Questa procedura realizza il principio di dev/prod parity, cioè la riduzione delle differenze tra ambiente di sviluppo, ambiente di test e ambiente di produzione. Non significa che gli ambienti siano identici in ogni dettaglio, ma che usano gli stessi Dockerfile e la stessa descrizione dei servizi. In questo modo possiamo scoprire già nella pipeline errori che altrimenti comparirebbero sul server.

Il collegamento con il principio 12-Factor è la configurazione separata dal codice e la riproducibilità·°del processo. Le variabili d'ambiente, come `VITE_API_URL`, non vengono confuse con il codice applicativo; le dipendenze sono definite dai file di progetto; la build è automatizzata e ripetibile.

La slide sul deployment mostra la distribuzione multi-cloud. Il frontend viene pubblicato su Vercel e il backend su Azure App Service. Quando il codice frontend viene aggiornato su GitHub, Vercel riceve il webhook, esegue la build e pubblica la nuova versione della SPA.

Il backend è un'applicazione ASP.NET Core eseguita su Azure App Service nella regione Sweden Central. App Service è un servizio PaaS: non dobbiamo amministrare direttamente il sistema operativo, ma dobbiamo configurare runtime, porta, variabili d'ambiente e accesso al database.

Il frontend e il backend comunicano con HTTP/REST. Il ruolo di `VITE_API_URL` è quindi fondamentale: durante la build su Vercel viene impostato l'indirizzo pubblico delle API Azure. Se questo valore fosse ancora `localhost`, il browser dell'utente cercherebbe il backend sul computer dell'utente e l'applicazione non funzionerebbe.

Bisogna considerare anche CORS. Poiché·°frontend e backend hanno origini diverse, l'API deve autorizzare l'origine del frontend. CORS è un controllo del browser che permette o blocca richieste provenienti da un dominio diverso; configurarlo correttamente evita sia il blocco delle chiamate legittime sia l'apertura indiscriminata dell'API.

Il flusso completo è quindi: sviluppatore crea un feature branch, apre una Pull Request, GitHub Actions esegue restore, build e validazione Docker, e dopo l'integrazione i provider cloud possono eseguire il deployment. La prossima parte spiegherà·°come vengono gestiti stato, autenticazione e scalabilità·°.
