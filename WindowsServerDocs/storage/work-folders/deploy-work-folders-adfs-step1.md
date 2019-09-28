---
title: Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web - Passaggio 1, configurare AD FS
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 10/18/2018
ms.assetid: 938cdda2-f17e-4964-9218-f5868fd96735
ms.openlocfilehash: 0920d091d6e8b5f3db9bf945a966fdd577918179
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365792"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-1-set-up-ad-fs"></a>Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: Passaggio 1, configurazione AD FS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive il primo passaggio nella distribuzione di Cartelle di lavoro con Active Directory Federation Services (AD FS) e Proxy applicazione Web. È possibile trovare gli altri passaggi di questo processo negli argomenti seguenti:  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Panoramica](deploy-work-folders-adfs-overview.md)  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 2, AD FS lavoro post-configurazione @ no__t-0  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 3: configurare cartelle di lavoro @ no__t-0  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 4: configurare il proxy dell'applicazione Web @ no__t-0  
  
-   Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 5: configurare i client @ no__t-0  
  
> [!NOTE]
>   Le istruzioni descritte in questa sezione si riferiscono a un ambiente Windows Server 2019 o Windows Server 2016. Se usi Windows Server 2012 R2, segui le [istruzioni di Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Per configurare AD FS per l'uso con le Cartelle di lavoro, utilizza le seguenti procedure.  
  
## <a name="pre-installment-work"></a>Operazione di pre\-installazione  
Se desideri convertire l'ambiente di test che stai configurando con queste istruzioni per la produzione, ci sono due operazioni che potresti voler eseguire prima di iniziare:  
  
-   Configurare un account amministratore di dominio Active Directory da utilizzare per eseguire il servizio AD FS.
  
-   Ottenere un certificato SSL (Secure Sockets Layer) con nome alternativo del soggetto (SAN) per l'autenticazione del server. Per l'esempio di test, utilizzerai un certificato autofirmato, ma per la produzione ne dovresti utilizzare uno attendibile pubblicamente.  
  
Ottenere questi elementi può richiedere del tempo, a seconda delle norme della propria azienda, quindi può essere utile avviare il processo di richiesta per gli elementi prima di iniziare a creare l'ambiente di test.  
  
Esistono molte autorità di certificazione commerciali (CA) da cui è possibile acquistare il certificato. È possibile trovare un elenco delle autorità di certificazione considerate attendibili da Microsoft nell' [articolo della Knowledge Base 931125](https://support.microsoft.com/kb/931125). In alternativa, è possibile ottenere un certificato da una CA dell'organizzazione (enterprise) della propria azienda.  
  
Per l'ambiente di test, utilizzerai un certificato autofirmato creato da uno degli script forniti.  
  
> [!NOTE]  
> AD FS non supporta i certificati Cryptography Next Generation (CNG), ovvero non è possibile creare il certificato autofirmato utilizzando il cmdlet New-SelfSignedCertificate di Windows PowerShell. Tuttavia, è possibile usare lo script makecert.ps1 incluso nel post di blog [Distribuire Cartelle di lavoro con Active Directory Federation Services e Proxy applicazione Web](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap). Questo script crea un certificato auto\-firmato che funziona con AD FS e richiede i nomi alternativi del soggetto (SAN) che saranno necessari per creare il certificato.  
  
Successivamente, esegui l'operazione aggiuntiva di pre-installazione descritta nelle sezioni seguenti.  
  
### <a name="create-an-ad-fs-self-signed-certificate"></a>Creare un certificato AD FS auto\-firmato  
Per creare un certificato AD FS autofirmato, segui questi passaggi:  
  
1.  Scarica gli script forniti nel post di blog [Distribuzione di Cartelle di lavoro con Active Directory Federation Services e Proxy dell'applicazione Web](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap) e quindi copia il file makecert.ps1 sul computer AD FS.  
  
2.  Apri una finestra di Windows PowerShell con privilegi di amministratore.  
  
3.  Imposta i criteri di esecuzione su senza restrizioni:  
  
    ```powershell  
    Set-ExecutionPolicy –ExecutionPolicy Unrestricted   
    ```  
  
4.  Passa alla directory in cui è stato copiato lo script.  
  
5.  Esegui lo script makecert:  
  
    ```powershell  
    .\makecert.ps1  
    ```  
  
6.  Quando viene richiesto di cambiare il certificato soggetto, immetti il nuovo valore per il soggetto. In questo esempio, il valore è **blueadfs.contoso.com**.  
  
7.  Quando viene richiesto di immettere i nomi alternativi del soggetto (SAN), premi Y e quindi immetti i nomi alternativi del soggetto (SAN), uno per volta.  
  
    Per questo esempio, digita **blueadfs.contoso.com** e premi INVIO, quindi digita **2016-adfs.contoso.com** e premi INVIO, quindi digita **enterpriseregistration.contoso.com** e premi INVIO.  
  
    Dopo avere immesso tutti i nomi alternativi del soggetto (SAN), premi INVIO su una riga vuota.  
  
8.  Quando viene richiesto di installare i certificati nell'archivio delle autorità di certificazione radice attendibili, premi Y.  
  
Il certificato AD FS deve essere un certificato SAN con i valori seguenti:  

-   nome.dominio del servizio AD FS

-   enterpriseregistration.dominio

-   nome.dominio del server AD FS

Nell'esempio di test, i valori sono:  
  
-   **blueadfs.contoso.com**
  
-   **enterpriseregistration.contoso.com**
  
-   **2016-adfs.contoso.com**
  
La SAN enterpriseregistration è necessaria per Workplace Join.  
  
### <a name="set-the-server-ip-address"></a>Imposta l'indirizzo IP del server  
Modifica l'indirizzo IP del server in un indirizzo IP statico. Per l'esempio di test, usare la classe IP A, che è 192.168.0.160/subnet mask: 255.255.0.0/gateway predefinito: 192.168.0.1/DNS preferito: 192.168.0.150 (indirizzo IP del controller di dominio @ no__t-0.  
  
## <a name="install-the-ad-fs-role-service"></a>Installare il servizio ruolo AD FS  
Per installare AD FS, esegui le seguenti operazioni:  
  
1.  Accedi al computer fisico o alla macchina virtuale nel quale prevedi di installare AD FS, apri **Server Manager**, e avvia l'Aggiunta guidata ruoli e funzionalità.  
  
2.  Nella pagina **Ruoli server** seleziona il ruolo **Active Directory Federation Services** e quindi fai clic su **Avanti**.  
  
3.  Nella pagina **Active Directory Federation Services (AD FS)** , verrà visualizzato un messaggio in cui è indicato che il ruolo Proxy dell'applicazione Web non può essere installato nello stesso computer di AD FS. Fare clic su **Avanti**.  
  
4.  Fai clic su **Installa** nella pagina di conferma.  
  
Per eseguire l'installazione equivalente di AD FS tramite Windows PowerShell, usa questi comandi:  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation –IncludeManagementTools  
```  
  
## <a name="configure-ad-fs"></a>Configurare ADFS  
Successivamente, configura AD FS utilizzando Server Manager o Windows PowerShell.  
  
### <a name="configure-ad-fs-by-using-server-manager"></a>Configurare AD FS utilizzando Server Manager  
Per configurare AD FS utilizzando Server Manager, segui questi passaggi:  
  
1.  Aprire Server Manager.  
  
2.  Fai clic sul flag **Notifiche** nella parte superiore della finestra di Server Manager e quindi fai clic su **Configurare il servizio federativo nel server**.  
  
3.  Verrà avviata la Configurazione guidata di Active Directory Federation Services. Nella pagina **Connessione ad Active Directory Domain Services**, immetti l'account amministratore di dominio che desideri utilizzare come account di AD FS e fai clic su **Avanti**.  
  
4.  Nella pagina **Impostazione proprietà del servizio**, immetti il nome del soggetto del certificato SSL per la comunicazione di AD FS. Nell'esempio di test, questo è **blueadfs.contoso.com**.  
  
5.  Immetti il nome servizio federativo. Nell'esempio di test, questo è **blueadfs.contoso.com**. Fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Il nome servizio federativo non deve utilizzare il nome di un server esistente nell'ambiente. Se utilizzi il nome di un server esistente, l'installazione di AD FS avrà esito negativo e dovrà essere riavviata.  
  
6.  Nella pagina **Impostazione account servizio**, immetti il nome che desideri utilizzare per l'account del servizio gestito. Per l'esempio di test, seleziona **Creare un account del servizio gestito del gruppo**e in **Nome account**, immetti **ADFSService**. Fare clic su **Avanti**.  
  
7.  Nella pagina **Impostazione database di configurazione**, seleziona **Creare un database nel server mediante il database interno di Windows** e fai clic su **Avanti**.  
  
8.  La pagina **Verifica opzioni** mostra una panoramica delle opzioni selezionate. Fare clic su **Avanti**.  
  
9. La pagina **Controlli dei prerequisiti** indica se tutti i controlli dei prerequisiti sono riusciti. Se non sono presenti problemi, fai clic su **Configurazione**.  
  
    > [!NOTE]  
    > Se hai usato il nome del server AD FS o di un qualsiasi altro computer esistente per il nome servizio federativo, viene visualizzato un messaggio di errore. È necessario avviare l'installazione e scegliere un nome diverso dal nome di un computer esistente.  
  
10. Quando la configurazione viene completata correttamente, la pagina **Risultati** conferma che AD FS è stato configurato correttamente.  
  
### <a name="configure-ad-fs-by-using-powershell"></a>Configurare AD FS utilizzando PowerShell  
Per eseguire la configurazione equivalente di AD FS utilizzando Windows PowerShell, utilizza i comandi seguenti.  
  
Per installare AD FS:  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation -IncludeManagementTools   
```  
  
Per creare l'account del servizio gestito:  
  
```powershell  
New-ADServiceAccount "ADFSService"-Server 2016-DC.contoso.com -Path "CN=Managed Service Accounts,DC=Contoso,DC=COM" -DNSHostName 2016-ADFS.contoso.com -ServicePrincipalNames HTTP/2016-ADFS,HTTP/2016-ADFS.contoso.com  
```  
  
Dopo aver configurato AD FS, è necessario impostare una farm AD FS utilizzando l'account del servizio gestito creato nel passaggio precedente e il certificato creato nei passaggi di pre-configurazione.  
  
Per impostare una farm AD FS:  
  
```powershell  
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match blueadfs.contoso.com} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
Install-ADFSFarm -CertificateThumbprint $thumbprint -FederationServiceDisplayName "Contoso Corporation" –FederationServiceName blueadfs.contoso.com -GroupServiceAccountIdentifier contoso\ADFSService$ -OverwriteConfiguration -ErrorAction Stop  
```  
  
Passaggio successivo: Cartelle di lavoro [Deploy con AD FS e proxy applicazione Web: Passaggio 2, AD FS lavoro post-configurazione @ no__t-0  
  
## <a name="see-also"></a>Vedere anche  
[Panoramica di cartelle di lavoro](Work-Folders-Overview.md)  
  

