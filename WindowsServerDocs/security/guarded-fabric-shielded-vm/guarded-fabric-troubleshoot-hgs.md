---
title: Risoluzione dei problemi del servizio sorveglianza Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 05888ce57b5b922fc330d9deab430d329fede69b
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222536"
---
# <a name="troubleshooting-the-host-guardian-service"></a>Risoluzione dei problemi del servizio sorveglianza Host

> Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono descritte le risoluzioni di problemi comuni riscontrati durante la distribuzione o l'uso di un server del servizio sorveglianza Host (HGS) in un'infrastruttura sorvegliata.
Se non si conosce la natura del problema, provare a eseguire prima la [sorvegliato diagnostica fabric](guarded-fabric-troubleshoot-diagnostics.md) nei server HGS e gli host Hyper-V per restringere la potenziale causa.

## <a name="certificates"></a>Certificati

HGS richiede certificati diversi per poter funzionare, tra cui la crittografia configurate dall'amministratore e firma del certificato, nonché un certificato di attestazione gestiti dal servizio HGS di se stesso.
Se questi certificati non sono configurati correttamente, HGS sarà possibile rispondere alle richieste dagli host Hyper-V che desiderano attestare o sbloccare le protezioni con chiave per le macchine virtuali schermate.
Le sezioni seguenti illustrano i problemi comuni relativi ai certificati configurati nel servizio HGS.

### <a name="certificate-permissions"></a>Autorizzazioni per certificati

HGS deve essere in grado di accedere a entrambe le chiavi pubbliche e private di crittografia e firma dei certificati aggiunti al servizio HGS per l'identificazione personale del certificato.
In particolare, il gruppo gestito necessita dell'accesso alle chiavi di account del servizio (gMSA) che esegue il servizio HGS.
Per trovare il gMSA utilizzato dal servizio HGS, eseguire il comando seguente in un prompt di PowerShell con privilegi elevato nel server HGS:

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

Come si concede l'accesso account gMSA per usare la chiave privata dipende da dove viene archiviata la chiave: nel computer come un file di certificato locale in un modulo di protezione hardware (HSM) o tramite un provider di archiviazione di chiavi personalizzate di terze parti.

#### <a name="grant-access-to-software-backed-private-keys"></a>Concedere l'accesso a chiavi private basate su software

Se si usa un certificato autofirmato o un certificato emesso da un'autorità di certificazione che è **non** archiviato in un modulo di protezione hardware o un provider di archiviazione chiavi personalizzate, è possibile modificare le autorizzazioni per chiavi private eseguendo il Procedura:

1. Gestore di certificati locale aprire (certlm. msc)
2. Espandere **personale > certificati** e trovare il certificato di firma o crittografia che si desidera aggiornare.
3. Fare clic con il pulsante destro del certificato e selezionare **tutte le attività > Gestisci chiavi Private**.
4. Fare clic su **Add** per concedere a un nuovo utente l'accesso alla chiave privata di crearli.
5. Nel selettore oggetti, immettere il nome dell'account gMSA per HGS disponibili in precedenza, quindi fare clic su **OK**.
6. Verificare il gMSA abbia **lettura** accesso al certificato.
7. Fare clic su **OK** per chiudere la finestra di autorizzazione.

Se si eseguono HGS in Server Core o gestisce il server in modalità remota, non sarà in grado di gestire le chiavi private tramite il gestore di certificati locale.
Al contrario, sarà necessario scaricare il [modulo di PowerShell di strumenti dell'infrastruttura protetta](https://www.powershellgallery.com/packages/GuardedFabricTools) che consentirà di gestire le autorizzazioni in PowerShell.

1. Aprire una console di PowerShell con privilegi elevata nel computer Server Core o usare la comunicazione remota di PowerShell con un account che disponga delle autorizzazioni di amministratore locale nel servizio HGS.
2. Eseguire i comandi seguenti per installare il modulo PowerShell di strumenti dell'infrastruttura protetta e concedere all'account gMSA l'accesso alla chiave privata.

```powershell
$certificateThumbprint = '<ENTER CERTIFICATE THUMBPRINT HERE>'

# Install the Guarded Fabric Tools module, if necessary
Install-Module -Name GuardedFabricTools -Repository PSGallery

# Import the module into the current session
Import-Module -Name GuardedFabricTools

# Get the certificate object
$cert = Get-Item "Cert:\LocalMachine\My\$certificateThumbprint"

# Get the gMSA account name
$gMSA = (Get-IISAppPool -Name KeyProtection).ProcessModel.UserName

# Grant the gMSA read access to the certificate
$cert.Acl = $cert.Acl | Add-AccessRule $gMSA Read Allow
```

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>Concedere l'accesso a moduli di protezione hardware o personalizzato supportato da provider le chiavi private

Se le chiavi private del certificato sono supportate da un modulo di protezione hardware (HSM) o un provider personalizzato di archiviazione chiavi (KSP), il modello di autorizzazione varia in base al fornitore del software specifico.
Per ottenere risultati ottimali, consultare la documentazione del fornitore o supporterà il sito per informazioni sulla chiave privata come vengono gestite le autorizzazioni per il dispositivo/software specifico.

Alcuni moduli di protezione hardware non supportano la concessione dell'accesso degli account utente specifico per una chiave privata. piuttosto, consentono all'account computer l'accesso a tutte le chiavi in un set di chiavi specifico.
Per questi dispositivi, è in genere sufficiente concedere l'accesso al computer per le chiavi e HGS sarà in grado di usare tale connessione.

**Suggerimenti per moduli di protezione hardware**

Di seguito sono le opzioni di configurazione suggerito per una corretta chiavi basate su moduli di protezione hardware di uso con HGS basato su Microsoft e le esperienze dei suoi partner.
Questi suggerimenti vengono forniti per comodità e non sono necessariamente essere corretta al momento della lettura, né vengono approvate dai produttori di hardware.
Per informazioni precise relative al dispositivo specifico, se si hanno ulteriori domande, contattare il produttore di modulo di protezione hardware.

Serie di marchio/modulo di protezione hardware      | Suggerimento
----------------------|-------------
Gemalto SafeNet       | Verificare che la proprietà di utilizzo chiave nel file di richiesta di certificato è impostata su messaggi 0xa0, consentendo il certificato da utilizzare per la firma e crittografia. Inoltre, è necessario concedere all'account gMSA *leggere* accesso alla chiave privata usando lo strumento di gestione di certificati locale (vedere la procedura descritta in precedenza).
nCipher nShield        | Assicurarsi che ogni nodo del servizio HGS ha accesso all'ambiente di sicurezza contenente le chiavi di firma e crittografia. Non devi configurare gMSA specifiche autorizzazioni.
Utimaco CryptoServers | Verificare che la proprietà di utilizzo chiave nel file di richiesta di certificato è impostata su 0x13, consentendo il certificato da usare per la crittografia, decrittografia e firma.

### <a name="certificate-requests"></a>Richieste di certificati

Se si usa un'autorità di certificazione per emettere i certificati in un ambiente di infrastruttura a chiave pubblica (PKI), è necessario assicurarsi che la richiesta del certificato include i requisiti minimi per l'utilizzo HGS di tali chiavi.

**I certificati di firma**

Proprietà CSR | Valore obbligatorio
-------------|---------------
Algoritmo    | RSA
Dimensione della chiave     | Almeno 2048 bit
Utilizzo chiavi    | Firma/Sign/DigitalSignature

**Certificati di crittografia**

Proprietà CSR | Valore obbligatorio
-------------|---------------
Algoritmo    | RSA
Dimensione della chiave     | Almeno 2048 bit
Utilizzo chiavi    | Crittografia/crittografare/DataEncipherment

**I modelli di servizi certificati Active Directory**

Se si usa modelli di certificato di servizi di certificati Active Directory (ADCS) per creare i certificati che è consigliabile usano un modello con le impostazioni seguenti:

Proprietà modello di servizi certificati Active Directory | Valore obbligatorio
-----------------------|---------------
Categoria provider      | Provider di archiviazione chiavi
Nome dell'algoritmo         | RSA
Dimensione minima della chiave       | 2048
Scopo                | Firma e crittografia
Estensione Utilizzo chiave    | Crittografia di dati chiave di crittografia, firma digitale ("Consenti la crittografia dei dati utente")


### <a name="time-drift"></a>Sfasamento di tempo

Se l'ora del server è stato deviato in modo significativo da quello degli altri nodi HGS o host Hyper-V nell'infrastruttura protetta, potrebbero verificarsi problemi con la validità del certificato del firmatario l'attestazione.
Il certificato del firmatario l'attestazione viene creato e rinnovato dietro le quinte nel servizio HGS e viene usato per firmare i certificati di integrità per gli host sorvegliati emessi dal servizio di attestazione.

Per aggiornare il certificato del firmatario attestation, eseguire il comando seguente in un prompt di PowerShell con privilegi elevato.

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

In alternativa, è possibile eseguire manualmente l'attività pianificata, aprendo **utilità di pianificazione** (Taskschd. msc), passando a **libreria utilità di pianificazione > Microsoft > Windows > HGSServer** e l'esecuzione di attività denominata **AttestationSignerCertRenewalTask**.

## <a name="switching-attestation-modes"></a>Cambio di modalità di attestazione

Se si passa HGS dalla modalità TPM per la modalità Active Directory o viceversa usando il [Set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet, potrebbe volerci fino a 10 minuti per ogni nodo nel cluster per avviare l'applicazione la nuova modalità di attestazione HGS.
Questo comportamento è normale.
Si consiglia di non rimuovere tutti i criteri che consente agli host la modalità di attestazione precedente fino a quando non è stato verificato che tutti gli host sono attestare correttamente usando la nuova modalità di attestazione.

**Problema noto durante il passaggio da TPM per la modalità Active Directory**

Se è stato inizializzato il cluster HGS in modalità TPM e in seguito si passa alla modalità di Active Directory, è presente un problema noto che impedirà altri nodi nel cluster del servizio HGS di passaggio per la nuova modalità di attestazione.
Per assicurarsi che tutti i server HGS applicano la modalità di attestazione corretta, eseguire `Set-HgsServer -TrustActiveDirectory` **in ogni nodo** del cluster di HGS.
Questo problema non si applica se si passa dalla modalità TPM per la modalità Active *e* il cluster è stato originariamente configurato in modalità Active Directory.

È possibile verificare la modalità di attestazione del server HGS eseguendo [Get-HgsServer](https://technet.microsoft.com/library/mt652162.aspx).

## <a name="memory-dump-encryption-policies"></a>Criteri di crittografia di dump di memoria

Se si sta provando a configurare i criteri di crittografia di dump di memoria e non viene visualizzato il valore predefinito HGS dump criteri (Hgs\_NoDumps, Hgs\_DumpEncryption e Hgs\_DumpEncryptionKey) o il cmdlet di criterio di dump ( Add-HgsAttestationDumpPolicy), è probabile che non è l'aggiornamento cumulativo più recente installata.
Per risolvere il problema, [aggiornare il server HGS](guarded-fabric-manage-hgs.md#patching-hgs) per l'aggiornamento cumulativo più recente Windows e [attivare i nuovi criteri di attestazione](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation).
Assicurarsi di che aggiornare gli host Hyper-V per lo stesso aggiornamento cumulativo prima di attivare i nuovi criteri di attestazione, come gli host che non dispongono di nuove funzionalità di crittografia dump installata probabilmente avrà esito negativo l'attestazione una volta attivato il criterio HGS.

## <a name="endorsement-key-certificate-error-messages"></a>Messaggi di errore di verifica dell'autenticità certificato di chiave

Durante la registrazione di un host usando il [Add-HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) cmdlet, due identificatori TPM vengono estratte dal file di identificatore di piattaforma fornita: il certificato di chiave di verifica dell'autenticità (EKcert) e la chiave di verifica dell'autenticità pubblico (EKpub).
Il EKcert identifica il produttore del TPM, che fornisce garanzie che il TPM sia autentico e prodotti attraverso la catena di fornitura normale.
In modo univoco il EKpub identifica tale specifiche TPM ed è una delle misure che HGS Usa per concedere a un host di macchine virtuali schermate di accesso per l'esecuzione.

Si riceverà un errore durante la registrazione di un host TPM, se una delle due condizioni sono vere:
1. Il file dell'identificatore della piattaforma **non** contenga un certificato di chiave di verifica dell'autenticità
2. Il file dell'identificatore della piattaforma contiene un certificato di chiave di verifica dell'autenticità, ma tale certificato viene **non è attendibile** nel sistema

Alcuni produttori TPM non includono EKcerts nella loro TPM.
Se si ritiene che questo è il caso con il TPM, verificare con l'OEM che il TPM non devono avere un EKcert e usare il `-Force` flag per registrare manualmente l'host con HGS.
Se il TPM deve avere un EKcert, ma non viene trovato nel file di identificatore di piattaforma, assicurarsi di usare una console PowerShell come amministratore (elevati) quando si esegue [Get-PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier) nell'host.

Se hai ricevuto l'errore che non è considerato attendibile il EKcert, assicurarsi di aver [installato il pacchetto di certificati radice TPM attendibili](guarded-fabric-install-trusted-tpm-root-certificates.md) in ogni server HGS e che il certificato radice per il fornitore del TPM è presente in locale del computer  **TrustedTPM\_RootCA** archiviare. Eventuali certificati intermedi applicabili devono anche essere installati nel **TrustedTPM\_IntermediateCA** viene archiviato nel computer locale.
Dopo aver installato i certificati radice e intermedi, dovrebbe essere possibile eseguire `Add-HgsAttestationTpmHost` correttamente.
