---
title: Risoluzione dei problemi relativi al servizio sorveglianza host
ms.prod: windows-server
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 4cbbb41b965a44b6c81b58adc94990bb4d6af046
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856404"
---
# <a name="troubleshooting-the-host-guardian-service"></a>Risoluzione dei problemi relativi al servizio sorveglianza host

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive le risoluzioni ai problemi comuni riscontrati durante la distribuzione o il funzionamento di un server del servizio sorveglianza host (HGS) in un'infrastruttura sorvegliata.
Se non si è certi della natura del problema, provare prima di tutto a eseguire la [diagnostica dell'infrastruttura sorvegliata](guarded-fabric-troubleshoot-diagnostics.md) sui server HGS e sugli host Hyper-V per circoscrivere le possibili cause.

## <a name="certificates"></a>Certificati

Per il funzionamento di HGS sono necessari diversi certificati, tra cui la crittografia e il certificato di firma configurati dall'amministratore, nonché un certificato di attestazione gestito da HGS stesso.
Se questi certificati non sono configurati correttamente, HGS non sarà in grado di gestire le richieste provenienti da host Hyper-V che desiderano attestare o sbloccare le protezioni con chiave per le macchine virtuali schermate.
Le sezioni seguenti riguardano i problemi comuni relativi ai certificati configurati in HGS.

### <a name="certificate-permissions"></a>Autorizzazioni per certificati

HGS deve essere in grado di accedere alle chiavi pubbliche e private dei certificati di crittografia e firma aggiunti a HGS dall'identificazione personale del certificato.
In particolare, l'account del servizio gestito del gruppo (gMSA) che esegue il servizio HGS richiede l'accesso alle chiavi.
Per trovare il gMSA usato da HGS, eseguire il comando seguente in un prompt di PowerShell con privilegi elevati nel server HGS:

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

La modalità di concessione dell'accesso all'account gMSA per l'uso della chiave privata dipende dalla posizione in cui è archiviata la chiave: nel computer come file di certificato locale, in un modulo di protezione hardware (HSM) o tramite un provider di archiviazione chiavi personalizzato di terze parti.

#### <a name="grant-access-to-software-backed-private-keys"></a>Concedi l'accesso alle chiavi private con supporto software

Se si usa un certificato autofirmato o un certificato rilasciato da un'autorità di certificazione che **non** è archiviato in un modulo di protezione hardware o in un provider di archiviazione chiavi personalizzato, è possibile modificare le autorizzazioni per la chiave privata attenendosi alla procedura seguente:

1. Aprire Gestione certificati locale (certlm. msc)
2. Espandere **Personal > Certificates** e individuare il certificato di firma o di crittografia che si desidera aggiornare.
3. Fare clic con il pulsante destro del mouse sul certificato e scegliere **tutte le attività > Gestisci chiavi private**.
4. Fare clic su **Aggiungi** per concedere a un nuovo utente l'accesso alla chiave privata del crearli.
5. In selezione oggetti immettere il nome dell'account gMSA per HGS trovato in precedenza, quindi fare clic su **OK**.
6. Verificare che gMSA disponga dell'accesso in **lettura** al certificato.
7. Fare clic su **OK** per chiudere la finestra autorizzazioni.

Se si esegue HGS in Server Core o si gestisce il server in modalità remota, non sarà possibile gestire le chiavi private usando Gestione certificati locale.
Sarà invece necessario scaricare il modulo di PowerShell per gli [strumenti di infrastruttura sorvegliati](https://www.powershellgallery.com/packages/GuardedFabricTools) , che consente di gestire le autorizzazioni in PowerShell.

1. Aprire una console di PowerShell con privilegi elevati nel computer server core o usare la comunicazione remota di PowerShell con un account che disponga di autorizzazioni di amministratore locale in HGS.
2. Eseguire i comandi seguenti per installare il modulo di PowerShell per gli strumenti dell'infrastruttura sorvegliata e concedere all'account gMSA l'accesso alla chiave privata.

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

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>Concessione dell'accesso a HSM o chiavi private personalizzate supportate dal provider

Se le chiavi private del certificato sono supportate da un modulo di protezione hardware (HSM) o da un provider di archiviazione chiavi (KSP) personalizzato, il modello di autorizzazione dipenderà dal fornitore di software specifico.
Per ottenere risultati ottimali, consultare la documentazione o il sito di supporto del fornitore per informazioni sul modo in cui vengono gestite le autorizzazioni per la chiave privata per il dispositivo o il software specifico.
In tutti i casi, il gMSA usato da HGS richiede autorizzazioni di *lettura* per le chiavi private di crittografia, firma e certificato di comunicazione, in modo che sia possibile eseguire operazioni di firma e crittografia.

Alcuni moduli di sicurezza hardware non supportano la concessione di account utente specifici per l'accesso a una chiave privata. ma consentono all'account del computer di accedere a tutte le chiavi in un set di chiavi specifico.
Per tali dispositivi, è in genere sufficiente fornire al computer l'accesso alle chiavi e HGS sarà in grado di sfruttare tale connessione.

**Suggerimenti per HSM**

Di seguito sono riportate le opzioni di configurazione consigliate che consentono di usare correttamente le chiavi supportate da HSM con HGS in base a Microsoft e ai relativi partner.
Questi suggerimenti vengono forniti per praticità e non è garantito che siano corretti al momento della lettura, né sono stati approvati dai produttori del modulo di protezione hardware.
Per altre domande, contattare il produttore del modulo di protezione hardware per informazioni accurate relative al dispositivo specifico.

Marchio/serie HSM      | Suggerimento
----------------------|-------------
SafeNet Gemalto       | Verificare che la proprietà utilizzo chiave nel file di richiesta del certificato sia impostata su messaggi 0XA0, consentendo l'utilizzo del certificato per la firma e la crittografia. Inoltre, è necessario concedere all'account gMSA l'accesso in *lettura* alla chiave privata mediante lo strumento Gestione certificati locale (vedere i passaggi precedenti).
nShield nCipher        | Verificare che ogni nodo HGS disponga dell'accesso all'ambiente di sicurezza contenente le chiavi di firma e di crittografia. Potrebbe inoltre essere necessario concedere all'gMSA l'accesso in *lettura* alla chiave privata mediante Gestione certificati locale (vedere la procedura precedente).
CryptoServers Utimaco | Verificare che la proprietà utilizzo chiave nel file di richiesta del certificato sia impostata su 0x13, consentendo l'utilizzo del certificato per la crittografia, la decrittografia e la firma.

### <a name="certificate-requests"></a>Richieste di certificati

Se si usa un'autorità di certificazione per emettere i certificati in un ambiente di infrastruttura a chiave pubblica (PKI), è necessario assicurarsi che la richiesta di certificato includa i requisiti minimi per l'utilizzo di tali chiavi da parte di HGS.

**Certificati di firma**

Proprietà CSR | Valore obbligatorio
-------------|---------------
Algoritmo    | RSA
Dimensioni chiave     | Almeno 2048 bit
Utilizzo chiavi    | Firma/firma/DigitalSignature

**Certificati di crittografia**

Proprietà CSR | Valore obbligatorio
-------------|---------------
Algoritmo    | RSA
Dimensioni chiave     | Almeno 2048 bit
Utilizzo chiavi    | Crittografia/crittografia/DataEncipherment

**Modelli di Servizi certificati Active Directory**

Se si usano i modelli di certificato di Servizi certificati Active Directory (ADC) per creare i certificati, è consigliabile usare un modello con le impostazioni seguenti:

Proprietà modello ADC | Valore obbligatorio
-----------------------|---------------
Categoria provider      | Provider di archiviazione chiavi
Nome dell'algoritmo         | RSA
Dimensioni minime chiave       | 2048
Scopo                | Firma e crittografia
Estensione utilizzo chiavi    | Firma digitale, crittografia chiave, crittografia dati ("Consenti crittografia dei dati utente")


### <a name="time-drift"></a>Sfasamento temporale

Se l'ora del server si è trascinata in modo significativo da quella di altri nodi HGS o host Hyper-V nell'infrastruttura sorvegliata, è possibile che si verifichino problemi con la validità del certificato del firmatario di attestazione.
Il certificato del firmatario di attestazione viene creato e rinnovato dietro le quinte in HGS e viene usato per firmare i certificati di integrità emessi per gli host sorvegliati dal servizio di attestazione.

Per aggiornare il certificato del firmatario di attestazione, eseguire il comando seguente in un prompt di PowerShell con privilegi elevati.

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

In alternativa, è possibile eseguire manualmente l'attività pianificata aprendo **utilità di pianificazione** (taskschd. msc), passando alla **Libreria Utilità di pianificazione > Microsoft > Windows > HGSServer** ed eseguendo l'attività denominata **AttestationSignerCertRenewalTask**.

## <a name="switching-attestation-modes"></a>Cambio di modalità di attestazione

Se si passa HGS dalla modalità TPM alla modalità Active Directory o viceversa usando il cmdlet [set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) , potrebbero essere necessari fino a 10 minuti per ogni nodo nel cluster HGS per avviare l'applicazione della nuova modalità di attestazione.
Si tratta di un comportamento normale.
Si consiglia di non rimuovere i criteri che consentono agli host dalla modalità di attestazione precedente fino a quando non si è verificato che tutti gli host sono stati attestati correttamente usando la nuova modalità di attestazione.

**Problema noto durante il cambio da TPM alla modalità AD**

Se si intialized il cluster HGS in modalità TPM e successivamente si passa alla modalità Active Directory, esiste un problema noto che impedisce ad altri nodi del cluster HGS di passare alla nuova modalità di attestazione.
Per assicurarsi che tutti i server HGS applichino la modalità di attestazione corretta, eseguire `Set-HgsServer -TrustActiveDirectory` **in ogni nodo** del cluster HGS.
Questo problema non si applica se si passa dalla modalità TPM alla modalità AD *e* il cluster è stato originariamente configurato in modalità ad.

È possibile verificare la modalità di attestazione del server HGS eseguendo [Get-HgsServer](https://technet.microsoft.com/library/mt652162.aspx).

## <a name="memory-dump-encryption-policies"></a>Criteri di crittografia del dump della memoria

Se si sta provando a configurare i criteri di crittografia dei dump della memoria e non vengono visualizzati i criteri di dump predefiniti di HGS (HGS\_nodumps, HGS\_DumpEncryption e HGS\_DumpEncryptionKey) o il cmdlet dump Policy (Add-HgsAttestationDumpPolicy), è probabile che non sia installato l'aggiornamento cumulativo più recente.
Per risolvere il problema, [aggiornare il server HGS](guarded-fabric-manage-hgs.md#patching-hgs) alla versione più recente di Windows Update cumulativa e [attivare i nuovi criteri di attestazione](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation).
Assicurarsi di aggiornare gli host Hyper-V allo stesso aggiornamento cumulativo prima di attivare i nuovi criteri di attestazione, perché gli host in cui non sono installate le nuove funzionalità di crittografia dei dump avranno probabilmente esito negativo per l'attestazione dopo l'attivazione del criterio HGS.

## <a name="endorsement-key-certificate-error-messages"></a>Messaggi di errore del certificato chiave di verifica dell'autenticità

Quando si registra un host mediante il cmdlet [Add-HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) , vengono estratti due identificatori TPM dal file di identificatore della piattaforma fornito: il certificato della chiave di verifica dell'autenticità (EKcert) e la chiave di verifica dell'autenticità pubblica (EKpub).
EKcert identifica il produttore del TPM, assicurando che il TPM sia autentico e prodotto tramite la normale supply chain.
EKpub identifica in modo univoco il TPM specifico ed è una delle misure che HGS USA per concedere a un host l'accesso per l'esecuzione di VM schermate.

Se una delle due condizioni è vera, verrà visualizzato un errore durante la registrazione di un host TPM:
1. Il file dell'identificatore di piattaforma **non** contiene un certificato della chiave di verifica dell'autenticità
2. Il file dell'identificatore di piattaforma contiene un certificato della chiave di verifica dell'autenticità, ma il certificato **non è attendibile** nel sistema

Alcuni produttori di TPM non includono EKcerts nella TPMs.
Se si ritiene che questo sia il caso del TPM, verificare con l'OEM che TPMs non disponga di un EKcert e usare il flag `-Force` per registrare manualmente l'host con HGS.
Se il TPM deve avere un EKcert ma uno non è stato trovato nel file di identificatore della piattaforma, assicurarsi di usare una console di PowerShell amministratore (con privilegi elevati) quando si esegue [Get-PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier) nell'host.

Se è stato visualizzato un errore che indica che il EKcert non è attendibile, verificare di aver [installato il pacchetto di certificati radice TPM attendibile](guarded-fabric-install-trusted-tpm-root-certificates.md) in ogni server HGS e che il certificato radice per il fornitore del TPM sia presente nell'archivio **TrustedTPM\_rootca** del computer locale. È necessario installare anche eventuali certificati intermedi applicabili nell'archivio **TrustedTPM\_IntermediateCA** nel computer locale.
Dopo aver installato i certificati radice e intermedi, sarà possibile eseguire correttamente `Add-HgsAttestationTpmHost`.
