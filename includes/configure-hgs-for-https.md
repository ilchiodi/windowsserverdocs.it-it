In primo luogo, è possibile ottenere un certificato SSL per servizio HGS da un'autorità di certificazione. Ogni computer host sarà necessario considerare attendibile il certificato SSL, pertanto è consigliabile generare il certificato SSL dell'azienda pubblico chiave dell'infrastruttura o terze parti, autorità di certificazione. Qualsiasi certificato SSL in IIS è supportata dal servizio HGS, tuttavia **il nome del soggetto nel certificato deve corrispondere il nome completo del servizio HGS** (nome rete distribuita del cluster). Ad esempio, se il dominio HGS è "bastion.local" e il nome del servizio HGS è "hgs", il certificato SSL deve essere visualizzato per "hgs.bastion.local". È possibile aggiungere ulteriori nomi DNS per il campo nome alternativo oggetto del certificato se necessario.

Dopo che il protocollo SSL si dispone del certificato, aprire una sessione di PowerShell con privilegi elevata e fornire il percorso del certificato quando si esegue [Set-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/host-guardian-service/server/set-hgsserver):


```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

In alternativa, se è già stato installato il certificato nell'archivio certificati locale, è possibile farvi riferimento tramite identificazione personale:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

> [!IMPORTANT]
> Configurazione del servizio HGS con un certificato SSL non disabilita l'endpoint HTTP.
> Se si vuole consentire solo l'utilizzo dell'endpoint HTTPS, configurare Windows Firewall per bloccare le connessioni in ingresso alla porta 80.
> **Non modificare le associazioni di IIS** per siti Web HGS per rimuovere l'endpoint HTTP; non è supportata per eseguire questa operazione.
