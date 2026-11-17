# My Sorintian Team — Releases

Distribuzione pubblica di My Sorintian Team. Questo repository contiene installer, pacchetti dei servizi e metadati degli aggiornamenti.

- **Client**: Windows (installer `.exe`), macOS (`.dmg`) e Linux (`.AppImage` / `.deb`).
- **Backend** e **runner**: pacchetti separati `.tar.gz`, richiedono Node.js 24.15+ nella linea 24.x.
- Ogni componente ha versioni e canali **stable** / **preview** indipendenti. I tag sono `client-vVERSION`, `backend-vVERSION`, `runner-vVERSION`.
- I file in `channels/` sono aggiornati automaticamente solo dopo la pubblicazione dei pacchetti.

## Client

Scarica il pacchetto dalle [release](../../releases). Nel client apri **Impostazioni → Aggiornamenti**. Il controllo automatico avviene all'avvio e ogni sei ore. Il client scarica la nuova versione e propone il riavvio. I servizi locali hanno pulsanti separati per il controllo, il download e l'aggiornamento. Nessun account GitHub è richiesto.

L'anteprima `0.1.0-preview.2` precedente non contiene un updater: installa una volta la nuova versione. Le anteprime non firmate possono richiedere autorizzazione del sistema operativo; l'aggiornamento automatico macOS richiede una build firmata. Su Linux l'auto-installazione usa AppImage; i pacchetti deb si aggiornano tramite installazione del nuovo pacchetto.

## Servizi separati

Estrai il pacchetto e avvia `node host.cjs`. Imposta `MY_IT_TEAM_DATA_DIR` a una directory persistente distinta per ogni servizio.

- `node host.cjs status`: stato del gestore attivo.
- `node host.cjs check`: controlla la nuova release.
- `node host.cjs stage`: verifica e prepara l'aggiornamento.
- `node host.cjs update`: applica l'aggiornamento richiesto dall'operatore al gestore già attivo, senza avviare una seconda copia del servizio.

I comandi richiedono la stessa `MY_IT_TEAM_DATA_DIR` del processo attivo. Backend e runner controllano e scaricano in background, ma **non si riavviano autonomamente**. Un aggiornamento già scaricato viene applicato al successivo avvio del servizio. In alternativa l'operatore può usare la CLI o, per i servizi locali, i pulsanti del client.

Il gestore preserva i dati e ripristina il codice precedente se la nuova versione non supera l'avvio. Non avviare più gestori sulla stessa directory. Le migrazioni dati future devono essere retrocompatibili per consentire il ripristino.

Ogni release include `SHA256SUMS.txt`. Il codice sorgente e la pipeline di costruzione rimangono nel repository privato.
