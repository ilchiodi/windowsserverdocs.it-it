---
title: Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web - Passaggio 3, configurare Cartelle di lavoro
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 5a43b104-4d02-4d73-a385-da1cfb67e341
ms.openlocfilehash: ef76b87928e696586356c499367051ff0d0e9ab4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365775"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-3-set-up-work-folders"></a>Distribuire Cartelle di lavoro con AD FS e Proxy applicazione Web: passaggio 3, configurare Cartelle di lavoro

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive il terzo passaggio nella distribuzione di Cartelle di lavoro con Active Directory Federation Services (AD FS) e Proxy applicazione Web. È possibile trovare gli altri passaggi di questo processo negli argomenti seguenti:  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: Panoramica](deploy-work-folders-adfs-overview.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 1, configurazione AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 2 AD FS lavoro di post-configurazione](deploy-work-folders-adfs-step2.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 4, configurare il proxy dell'applicazione Web](deploy-work-folders-adfs-step4.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 5, configurare i client](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   Le istruzioni descritte in questa sezione si riferiscono a un ambiente Windows Server 2019 o Windows Server 2016. Se usi Windows Server 2012 R2, segui le [istruzioni di Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Per configurare Cartelle di lavoro, utilizza le procedure seguenti.  
  
## <a name="pre-installment-work"></a>Operazione di pre\-installazione  
Per installare Cartelle di lavoro, è necessario disporre di un server che venga aggiunto al dominio ed esegua Windows Server 2016. Il server deve avere una configurazione di rete valida.  
  
Nell'esempio di test, aggiungi il computer che eseguirà Cartelle di lavoro nel dominio Contoso e configura l'interfaccia di rete come descritto nelle sezioni seguenti. 

### <a name="set-the-server-ip-address"></a>Imposta l'indirizzo IP del server  
Modifica l'indirizzo IP del server in un indirizzo IP statico. Nell'esempio di test, utilizza la classe IP A, ovvero 192.168.0.170 / subnet mask: 255.255.0.0 / Gateway predefinito: 192.168.0.1 / DNS preferito: 192.168.0.150 (l'indirizzo IP del tuo controller di dominio). 
  
### <a name="create-the-cname-record-for-work-folders"></a>Creare il record di CNAME per Cartelle di lavoro  
Per creare il record di CNAME per Cartelle di lavoro, segui questi passaggi:  
  
1.  Nel tuo controller di dominio apri **Gestore DNS**.  
  
2.  Espandi la cartella Zone di ricerca diretta, fai clic con il tasto destro del mouse sul tuo dominio e fai clic su **Nuovo alias (CNAME)** .  
  
3.  Nella finestra **Nuovo Record di risorse**, nel campo **Nome Alias**, immetti l'alias per Cartelle di lavoro. Nell'esempio di test, si tratta di **workfolders**.  
  
4.  Nel campo **Nome di dominio completo**, il valore dovrebbe essere **workfolders.contoso.com**.  
  
5.  Nel campo **Nome di dominio completo dell'host di destinazione**, immetti il FQDN per il server di Cartelle di lavoro. Nell'esempio di test, si tratta di **2016-WF.contoso.com**.  
  
6.  Fare clic su **OK**.  
  
Per eseguire i passaggi equivalenti tramite Windows PowerShell, usa il seguente comando. Il comando deve essere eseguito sul controller di dominio.  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name workfolders -CName  -HostNameAlias 2016-wf.contoso.com   
```  
  
### <a name="install-the-ad-fs-certificate"></a>Installare il certificato AD FS  
Installa il certificato AD FS che è stato creato durante l'installazione di AD FS nell'archivio certificati del computer locale, procedendo come segue:  
  
1.  Fare clic su **Start**, quindi scegliere **Esegui**.  
  
2.  Tipo **MMC**.  
  
3.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
4.  Nella lista **Snap-in disponibili**, seleziona **Certificati**, quindi fai clic su **Aggiungi**. Verrà avviata la procedura guidata Snap-in certificati.  
  
5.  Seleziona **Account del computer** e quindi fai clic su **Avanti**.  
  
6.  Seleziona **Computer locale: (il computer su cui è in esecuzione questa console)** , quindi fai clic su **Fine**.  
  
7.  Fare clic su **OK**.  
  
8.  Espandi la cartella **Console Root\Certificates\(Local Computer) \Personal\Certificates**.  
  
9. Fai clic con il pulsante destro del mouse su **Certificati**, fai clic su **Tutte le attività**, quindi fai clic su **Importa**.  
  
10. Individuare la cartella contenente il certificato di AD FS e seguire le istruzioni della procedura guidata per importare il file e inserirlo nell'archivio certificati.

11. Espandi la cartella **Radice console\Certificati\(Computer locale)\Autorità di certificazione radice attendibili\Certificati**.  
  
12. Fai clic con il pulsante destro del mouse su **Certificati**, fai clic su **Tutte le attività**, quindi fai clic su **Importa**.  
  
13. Individuare la cartella contenente il certificato di AD FS e seguire le istruzioni della procedura guidata per importare il file e inserirlo nell'archivio Autorità di certificazione radice attendibili.  
  
### <a name="create-the-work-folders-self-signed-certificate"></a>Creare il certificato autofirmato di Cartelle di lavoro  
Per creare il certificato autofirmato di Cartelle di lavoro, segui questi passaggi:  
  
1.  Scarica gli script forniti nel post di blog [Distribuzione di Cartelle di lavoro con Active Directory Federation Services e Proxy applicazione Web](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap), quindi copia il file makecert.ps1 sul computer Cartelle di lavoro.  
  
2.  Apri una finestra di Windows PowerShell con privilegi di amministratore.  
  
3.  Imposta i criteri di esecuzione su senza restrizioni:  
  
    ```powershell  
    PS C:\temp\scripts> Set-ExecutionPolicy -ExecutionPolicy Unrestricted   
    ```  
  
4.  Passa alla directory in cui è stato copiato lo script.  
  
5.  Esegui lo script makeCert:  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1  
    ```  
  
6.  Quando viene richiesto di cambiare il certificato soggetto, immetti il nuovo valore per il soggetto. In questo esempio, il valore è **workfolders.contoso.com**.  
  
7.  Quando viene richiesto di immettere i nomi alternativi del soggetto (SAN), premi Y e quindi immetti i nomi alternativi del soggetto (SAN), uno per volta.  
  
    In questo esempio, digita **workfolders.contoso.com** e premi INVIO. Quindi digita **2016-WF.contoso.com** e premi INVIO.  
  
    Dopo avere immesso tutti i nomi alternativi del soggetto (SAN), premi INVIO su una riga vuota.  
  
8.  Quando viene richiesto di installare i certificati nell'archivio delle autorità di certificazione radice attendibili, premi Y.  
  
Il certificato Cartelle di lavoro deve essere un certificato SAN con i valori seguenti:  
  
-   **cartellelavoro**.**dominio**  
  
-   **nome del computer**.**dominio**  
  
Nell'esempio di test, i valori sono:  
  
-   **workfolders.contoso.com**  
  
-   **2016-WF.contoso.com**  
  
## <a name="install-work-folders"></a>Installare Cartelle di lavoro  
Per installare il ruolo di Cartelle di lavoro, segui questi passaggi:  
  
1.  Apri **Server Manager**, fai clic su **Aggiungi ruoli e funzionalità** e quindi fai clic su **Avanti**.  
  
2.  Nella pagina **Tipo di installazione**, seleziona **Installazione basata su ruoli o basata su funzionalità** e quindi fai clic su **Avanti**.  
  
3.  Nella pagina **Selezione dei server**, seleziona il server corrente e fai clic su **Avanti**.  
  
4.  Nella pagina **Ruoli server**, espandi **Servizi file e archiviazione**, espandi **Servizi file e iSCSI**, quindi seleziona **Cartelle di lavoro**.  
  
5.  Nella pagina **Aggiunta guidata ruoli e funzionalità**, fai clic su **Aggiungi funzionalità**, quindi fai clic su **Avanti**.  
  
6.  Nella pagina **Funzionalità**, fai clic su **Avanti**.  
  
7.  Nella pagina **Conferma** fare clic su **Installa**.  
  
## <a name="configure-work-folders"></a>Configurare Cartelle di lavoro  
Per configurare Cartelle di lavoro, segui questi passaggi:  
  
1.  Apri **Server Manager**.  
  
2.  Seleziona **Servizi file e archiviazione**, quindi seleziona **Cartelle di lavoro**.  
  
3.  Nella pagina **Cartelle di lavoro**, avvia la **Creazione guidata nuova condivisione di sincronizzazione**, quindi fai clic su **Avanti**.  
  
4.  Nella pagina **Server e il percorso**, seleziona il server in cui la condivisione di sincronizzazione verrà creata, immetti un percorso locale in cui verranno archiviati i dati di Cartelle di lavoro, quindi fai clic su **Avanti**.  
  
    Se il percorso non esiste, ti verrà richiesto di crearlo. Fare clic su **OK**.  
  
5.  Nella pagina **Struttura delle cartelle utente**, seleziona **Alias utente**, quindi fai clic su **Avanti**.  
  
6.  Nella pagina **Nome della condivisione di sincronizzazione**, immetti il nome per la condivisione di sincronizzazione. Nell'esempio di test, si tratta di **WorkFolders**. Fare clic su **Avanti**.  
  
7.  Nella pagina **Accesso di sincronizzazione**, aggiungi utenti o gruppi che avranno accesso alla nuova condivisione di sincronizzazione. Nell'esempio di test, concedi l'accesso a tutti gli utenti di dominio. Fare clic su **Avanti**.  
  
8.  Nella pagina **Criteri di protezione PC**, seleziona **Crittografa Cartelle di lavoro** e **Blocca automaticamente la pagina e richiedi una password**. Fare clic su **Avanti**.  
  
9. Nella pagina **Conferma**, fai clic su **Creazione** per completare il processo di configurazione.  
  
## <a name="work-folders-post-configuration-work"></a>Operazioni di post-configurazione Cartelle di lavoro  
Per completare la configurazione di Cartelle di lavoro, completa questi passaggi aggiuntivi:  
  
-   Associa il certificato Cartelle di lavoro alla porta SSL  
  
-   Configura Cartelle di lavoro per l'utilizzo dell'autenticazione di AD FS  
  
-   Esporta il certificato Cartelle di lavoro (se utilizzi un certificato autofirmato)  
  
### <a name="bind-the-certificate"></a>Associa il certificato  
Cartelle di lavoro comunica solo tramite SSL e deve avere il certificato autofirmato creato in precedenza (o emesso dalla tua autorità di certificazione) associato alla porta.  
  
Esistono due metodi che puoi utilizzare per associare il certificato alla porta tramite Windows PowerShell: i cmdlet IIS e netsh.  
  
#### <a name="bind-the-certificate-by-using-netsh"></a>Associare il certificato usando netsh  
Per utilizzare l'utilità di scripting dalla riga di comando di netsh in Windows PowerShell, è necessario inviare tramite pipe il comando a netsh. Il seguente script di esempio consente di trovare il certificato con soggetto **workfolders.contoso.com** e lo associa alla porta 443 utilizzando netsh:  
  
```powershell  
$subject = "workfolders.contoso.com"   
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
$Command = "http add sslcert ipport=0.0.0.0:443 certhash=$thumbprint appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY"  
$Command | netsh  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
#### <a name="bind-the-certificate-by-using-iis-cmdlets"></a>Associa il certificato utilizzando i cmdlet IIS  
Inoltre, puoi associare il certificato alla porta utilizzando i cmdlet di gestione IIS, che sono disponibili se sono installati gli strumenti e script di gestione IIS.  
  
> [!NOTE]  
> L'installazione degli strumenti di gestione IIS non abilita la versione completa di Internet Information Services (IIS) nel computer Cartelle di lavoro; abilita solo i cmdlet di gestione. Esistono alcuni vantaggi possibili per questa configurazione. Ad esempio, se stai cercando i cmdlet per fornire la funzionalità ottenuta da netsh. Quando il certificato è associato alla porta tramite il cmdlet New-WebBinding, l'associazione non è dipendente da IIS in alcun modo. Dopo aver eseguito l'associazione, è possibile anche rimuovere la funzionalità di Web-Mgmt-Console e il certificato resterà associato alla porta. È possibile verificare l'associazione tramite netsh digitando **netsh http show sslcert**.  
  
L'esempio seguente usa il cmdlet New-WebBinding per trovare il certificato con il soggetto **workfolders.contoso.com** e associarlo a una porta 443:  
  
```powershell  
$subject = "workfolders.contoso.com"  
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert =Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject } | sort $_.NotAfter -Descending | select -first 1   
$thumbprint = $cert.Thumbprint  
New-WebBinding -Name "Default Web Site" -IP * -Port 443 -Protocol https  
#The default IIS website name must be used for the binding. Because Work Folders uses Hostable Web Core and its own configuration file, its website name, 'ECSsite', will not work with the cmdlet. The workaround is to use the default IIS website name, even though IIS is not enabled, because the NewWebBinding cmdlet looks for a site in the default IIS configuration file.   
Push-Location IIS:\SslBindings  
Get-Item cert:\LocalMachine\MY\$thumbprint | new-item *!443  
Pop-Location  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
### <a name="set-up-ad-fs-authentication"></a>Configurare l'autenticazione di AD FS  
Per configurare Cartelle di lavoro per l'utilizzo di AD FS per l'autenticazione, segui questi passaggi:  
  
1.  Apri **Server Manager**.  
  
2.  Fai clic su **Server**, quindi seleziona il server Cartelle di lavoro nell'elenco.  
  
3.  Fai clic con il pulsante destro del mouse sul nome del server, quindi scegli **Impostazioni di Cartelle di lavoro**.  
  
4.  Nella finestra **Impostazioni di Cartelle di lavoro**, seleziona **Active Directory Federation Services** e digita l'URL del servizio federativo. Fare clic su **Applica**.  
  
    Nell'esempio di test, l'URL è **https://blueadfs.contoso.com** .  
  
Il cmdlet per eseguire la stessa attività tramite Windows PowerShell è:  
  
```powershell  
Set-SyncServerSetting -ADFSUrl "https://blueadfs.contoso.com"   
```  
  
Se stai configurando AD FS con certificati autofirmati, potresti ricevere un messaggio di errore che indica che l'URL del servizio federativo è errato, non raggiungibile o che un trust della relying party non è stato impostato.  
  
Questo errore può verificarsi anche se il certificato AD FS non è stato installato nel server di Cartelle di lavoro o se il CNAME per AD FS è non stato configurato correttamente. È necessario correggere questi problemi prima di procedere.  
  
### <a name="export-the-work-folders-certificate"></a>Esporta il certificato Cartelle di lavoro  
Il certificato Cartelle di lavoro autofirmato deve essere esportato in modo tale che tu possa installarlo in un secondo momento nei seguenti computer nell'ambiente di test:  
  
-   Il server utilizzato per Proxy dell'applicazione Web  
  
-   Il client Windows aggiunto al dominio  
  
-   Il client Windows non aggiunto al dominio  
  
Per esportare il certificato, segui gli stessi passaggi utilizzati per esportare il certificato AD FS in precedenza, come descritto in [Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: passaggio 2, lavoro post-configurazione di AD FS](deploy-work-folders-adfs-step2.md), ed esporta il certificato AD FS.  
  
Passaggio successivo: [Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: passaggio 4, configurare Proxy dell'applicazione Web](deploy-work-folders-adfs-step4.md)  
  
## <a name="see-also"></a>Vedi anche  
[Panoramica di cartelle di lavoro](Work-Folders-Overview.md)  
  

