Se è stata fornita eventuali certificati di HGS usando le identificazioni personali, verrà invitati a concedere l'accesso in lettura alla chiave privata di tali certificati HGS. In un server con esperienza Desktop installato, completare i passaggi seguenti:

1.  Aprire il gestore di certificati computer locale (**certlm. msc**)
2.  Trovare i certificati > fare doppio clic su > tutte le attività > Gestisci chiavi private
3.  Fare clic su **Aggiungi**.
4.  Nella finestra di selezione oggetti, fare clic su **tipi di oggetti** e abilitare **gli account del servizio**
5.  Immettere il nome dell'account del servizio indicato nel messaggio di avviso da `Initialize-HgsServer`
6.  Assicurarsi che gmsa disponga dell'accesso "Lettura" per la chiave privata.

In server core, è necessario scaricare un modulo di PowerShell per facilitare l'impostazione di autorizzazioni per chiavi private.

1.  Eseguire `Install-Module GuardedFabricTools` nel server HGS se dispone di connettività a Internet oppure eseguire `Save-Module GuardedFabricTools` in un altro computer e copia il modulo tramite il server HGS.
2.  Eseguire `Import-Module GuardedFabricTools`. Questo aggiungerà le proprietà aggiuntive al certificato gli oggetti individuati in PowerShell.
3.  Trovare l'identificazione digitale del certificato in PowerShell con `Get-ChildItem Cert:\LocalMachine\My`
4.  Aggiornare l'elenco ACL, sostituendo l'identificazione personale con quelli personalizzati e l'account gMSA nel codice seguente con l'account elencato nel messaggio di avviso di `Initialize-HgsServer`.

    ```powershell
    $certificate = Get-Item "Cert:\LocalMachine\1A2B3C..."
    $certificate.Acl = $certificate.Acl | Add-AccessRule "HgsSvc_1A2B3C" Read Allow
    ```

Se si usa moduli di protezione hardware certificati o i certificati archiviati in un provider di archiviazione delle chiavi di terze parti, questi passaggi potrebbero non essere applicabili all'utente. Consultare la documentazione del provider di archiviazione chiavi per informazioni su come gestire le autorizzazioni per la chiave privata. In alcuni casi, non è disponibile alcuna autorizzazione o l'autorizzazione viene fornita per l'intero computer durante l'installazione del certificato.

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->