---
ms.assetid: d2429185-9720-4a04-ad94-e89a9350cdba
title: Distribuzione di Cartelle di lavoro
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 6/24/2017
description: Come distribuire Cartelle di lavoro, tra cui l'installazione del ruolo del server, la creazione di condivisioni di sincronizzazione e di record DNS.
ms.openlocfilehash: d2ba117a021cfc7361c0f7c8df2ed9f3c4bc9d94
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792344"
---
# <a name="deploying-work-folders"></a>Distribuzione di Cartelle di lavoro

>Si applica a Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

In questo argomento vengono illustrati i passaggi necessari per la distribuzione di Cartelle di lavoro. Si suppone che le informazioni disponibili in [Pianificazione di una distribuzione di Cartelle di lavoro](plan-work-folders.md) siano già note.  
  
 Eseguire i passaggi seguenti per distribuire Cartelle di lavoro, un processo che può coinvolgere più server e tecnologie.  
  
> [!TIP]
>  La distribuzione di Cartelle di lavoro più semplice è costituita da un singolo file server (spesso denominato server di sincronizzazione) senza supporto per la sincronizzazione tramite Internet. Questa può essere una distribuzione utile per un laboratorio di testing oppure per la sincronizzazione di computer client che fanno parte di un dominio. Per creare una distribuzione semplice, è necessario eseguire almeno i passaggi seguenti: 
>  -   Passaggio 1: Ottenere i certificati SSL  
>  -   Passaggio 2: Creare record DNS 
>  -   Passaggio 3: Installare Cartelle di lavoro nei file server  
>  -   Passaggio 4: Associazione del certificato SSL nei server di sincronizzazione
>  -   Passaggio 5: Creare gruppi di sicurezza per Cartelle di lavoro  
>  -   Passaggio 7: Creare condivisioni di sincronizzazione per i dati degli utenti  
  
## <a name="step-1-obtain-ssl-certificates"></a>Passaggio 1: Ottenere i certificati SSL  
 Cartelle di lavoro usa HTTPS per sincronizzare i file tra i client di cartelle di lavoro e il server di cartelle di lavoro in modo sicuro. I requisiti dei certificati SSL utilizzati da Cartelle di lavoro sono i seguenti:  
  
- Il certificato deve essere emesso da un'autorità di certificazione attendibile. Per la maggior parte delle implementazioni di Cartelle di lavoro, è consigliabile utilizzare una CA pubblicamente attendibile, poiché i certificati verranno usati da dispositivi basati su Internet che non fanno parte del dominio.  
  
- Il certificato deve essere valido.  
  
- La chiave privata del certificato deve essere esportabile, poiché il certificato dovrà essere installato in più server.  
  
- Il nome del soggetto del certificato deve contenere l'URL pubblico di Cartelle di lavoro utilizzato per l'individuazione del servizio Cartelle di lavoro tramite Internet. L'URL deve avere il formato `workfolders.` *<nome_dominio>* .  
  
- Nel certificato devono essere presenti nomi alternativi del soggetto (SAN) che indicano il nome di server per ogni server di sincronizzazione in uso.

  Il [blog](https://blogs.technet.microsoft.com/filecab/2013/08/09/work-folders-certificate-management/) relativo alla Gestione dei certificati di Cartelle di lavoro fornisce informazioni aggiuntive sull'utilizzo dei certificati con le Cartelle di lavoro.
  
## <a name="step-2-create-dns-records"></a>Passaggio 2: Creare record DNS  
 Per fare in modo che gli utenti possano eseguire la sincronizzazione tramite Internet, è necessario creare un record host (A) nel DNS pubblico per consentire ai client Internet di risolvere l'URL di Cartelle di lavoro. Questo record DNS deve eseguire la risoluzione nell'interfaccia esterna del server del proxy inverso.  
  
 Nella rete interna, crea un record CNAME in DNS denominato cartellelavoro che esegue la risoluzione nel nome di dominio completo di un server Cartelle di lavoro. Quando i client di cartelle di lavoro usano l'individuazione automatica, l'URL usato per individuare il server di cartelle di lavoro è https:\//workfolders.domain.com. Se si prevede di utilizzare l'individuazione automatica, il record CNAME di cartellelavoro deve essere presente in DNS.  
  
## <a name="step-3-install-work-folders-on-file-servers"></a>Passaggio 3: Installare Cartelle di lavoro nei file server  
 È possibile installare Cartelle di lavoro in un server che fa parte di un dominio utilizzando Server Manager o Windows PowerShell in locale o in remoto in una rete. Questa operazione risulta utile nella se si configurano più server di sincronizzazione.  
  
Per distribuire il ruolo in Server Manager, eseguire le operazioni seguenti:  
  
1.  Avviare l'**Aggiunta guidata ruoli e funzionalità**.  
  
2.  Nella pagina **Selezione tipo di installazione** selezionare **Installazione basata su ruoli o basata su funzionalità**.  
  
3.  Nella pagina **Selezione server di destinazione** selezionare il server in cui si desidera installare Cartelle di lavoro.  
  
4.  Nella pagina **Selezione ruoli server** espandere **Servizi file e archiviazione**, espandere **Servizi file e iSCSI** e quindi selezionare **Cartelle di lavoro**.  
  
5.  Quando viene richiesto se si desidera installare **HWC (Hostable Web Core) di IIS**, fare clic su **OK** per installare la versione minima di Internet Information Services (IIS) richiesta da Cartelle di lavoro.  
  
6.  Fare clic su **Avanti** fino al termine della procedura guidata.

Per distribuire il ruolo mediante Windows PowerShell, utilizzare il cmdlet seguente:
  
```powershell  
Add-WindowsFeature FS-SyncShareService  
```

## <a name="step-4-binding-the-ssl-certificate-on-the-sync-servers"></a>Passaggio 4: Associazione del certificato SSL nei server di sincronizzazione
 Cartelle di lavoro installa HWC (Hostable Web Core) di IIS, un componente di IIS progettato per abilitare i servizi Web senza che sia necessaria un'installazione completa di IIS. Dopo aver installato HWC di IIS, è necessario associare il certificato SSL per il server al sito Web predefinito nel file server. HWC di IIS tuttavia non installa la console di gestione di IIS.

 È possibile associare il certificato all'interfaccia Web predefinita in due modi. Per entrambi è necessario installare la chiave privata per il certificato nell'archivio personale del computer.

- Utilizzare la console di gestione di IIS in un server in cui è installata. Dalla console connettersi al file server che si desidera gestire e quindi selezionare il sito Web predefinito per tale server. Il sito Web predefinito sarà disabilitato, ma sarà comunque possibile modificare le associazioni per il sito e selezionare il certificato da associare a tale sito Web.

- Utilizzare il comando netsh per associare il certificato all'interfaccia https del sito Web predefinito. Il comando è il seguente:

    ```
    netsh http add sslcert ipport=<IP address>:443 certhash=<Cert thumbprint> appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY
    ```

## <a name="step-5-create-security-groups-for-work-folders"></a>Passaggio 5: Creare gruppi di sicurezza per Cartelle di lavoro
 Prima di creare le condivisioni di sincronizzazione, un membro del gruppo Domain Admins o Enterprise Admins deve creare alcuni gruppi di sicurezza in Active Directory Domain Services (AD DS) per Cartelle di lavoro. È inoltre possibile delegare alcuni controlli come illustrato nel passaggio 6. I gruppi necessari sono i seguenti:

- Un gruppo per ogni condivisione di sincronizzazione per specificare a quali utenti è consentito eseguire la sincronizzazione con la condivisione di sincronizzazione

- Un gruppo per tutti gli amministratori di Cartelle di lavoro in modo che possano modificare un attributo in ogni oggetto utente che collega l'utente al server di sincronizzazione corretto (se si intende utilizzare più server di sincronizzazione)

  I gruppi devono adottare una convezione di denominazione standard e devono essere utilizzati solo per Cartelle di lavoro per evitare potenziali conflitti con altri requisiti di sicurezza.

  Per creare i gruppi di sicurezza appropriati, utilizzare la procedura seguente più volte, una per ogni condivisione di sincronizzazione e una per creare, se lo si desidera, un gruppo per gli amministratori di file server.

#### <a name="to-create-security-groups-for-work-folders"></a>Per creare gruppi di sicurezza per Cartelle di lavoro

1. Apri Server Manager in un computer Windows Server 2012 R2 o Windows Server 2016 in cui è installato Centro amministrativo di Active Directory.

2.  Dal menu **Strumenti** scegliere **Centro amministrativo di Active Directory**. Verrà visualizzato Centro amministrativo di Active Directory.

3.  Fare clic con il pulsante destro del mouse sul contenitore in cui si desidera creare il nuovo gruppo (ad esempio il contenitore Utenti del dominio o dell'unità organizzativa appropriata), scegliere **Nuovo** e quindi fare clic su **Gruppo**.

4.  Nella sezione **Gruppo** della finestra **Crea gruppo** specificare le impostazioni seguenti:

    -   Digitare il nome del gruppo di sicurezza in **Nome gruppo**, ad esempio: **HR Sync Share Users** o **Work Folders Administrators**.  
  
    -   In **Ambito del gruppo** fare clic su **Sicurezza** e quindi su **Globale**.  
  
5.  Nella sezione **Membri** fare clic su **Aggiungi**. Verrà visualizzata la finestra di dialogo Selezione utenti, computer, account servizio o gruppi.  
  
6.  Digita i nomi degli utenti o dei gruppi a cui desideri consentire l'accesso a una condivisione di sincronizzazione specifica (se stai creando un gruppo per controllare gli accessi a una condivisione di sincronizzazione) oppure digita i nomi degli amministratori di Cartelle di lavoro (se configurerai gli account utente per l'individuazione automatica del server di sincronizzazione appropriato), fai clic su **OK** e quindi di nuovo su **OK**.

Per creare un gruppo di sicurezza mediante Windows PowerShell, utilizzare i cmdlet seguenti:

```powershell  
$GroupName = "Work Folders Administrators"  
$DC = "DC1.contoso.com"  
$ADGroupPath = "CN=Users,DC=contoso,DC=com"  
$Members = "CN=Maya Bender,CN=Users,DC=contoso,DC=com","CN=Irwin Hume,CN=Users,DC=contoso,DC=com"  
  
New-ADGroup -GroupCategory:"Security" -GroupScope:"Global" -Name:$GroupName -Path:$ADGroupPath -SamAccountName:$GroupName -Server:$DC  
Set-ADGroup -Add:@{'Member'=$Members} -Identity:$GroupName -Server:$DC
```

## <a name="step-6-optionally-delegate-user-attribute-control-to-work-folders-administrators"></a>Passaggio 6: Delegare facoltativamente il controllo degli attributi utente agli amministratori di Cartelle di lavoro  
 Se si distribuiscono più server di sincronizzazione e si desidera fare in modo che gli utenti vengano indirizzati automaticamente al server di sincronizzazione corretto, è necessario aggiornare un attributo in ogni account utente in AD DS. Questa operazione richiede in genere che un membro del gruppo Domain Admins o Enterprise Admins aggiorni gli attributi, e può diventare fastidiosa se si aggiungono spesso utenti o li si sposta tra i server di sincronizzazione.  
  
 Per questo motivo, il membro del gruppo Domain Admins o Enterprise Admins può delegare il permesso di modificare la proprietà msDS-SyncServerURL degli oggetti utente al gruppo di amministratori di Cartelle di lavoro creato nel passaggio 5, come illustrato nella procedura seguente.  
  
#### <a name="delegate-the-ability-to-edit-the-msds-syncserverurl-property-on-user-objects-in-ad-ds"></a>Delegare il permesso di modificare la proprietà msDS-SyncServerURL degli oggetti utente in AD DS  
  
1.  Apri Server Manager in un computer Windows Server 2012 R2 o Windows Server 2016 in cui è installato Utenti e computer di Active Directory.  
  
2.  Dal menu **Strumenti** scegliere **Utenti e computer di Active Directory**. Verrà visualizzata la finestra di dialogo Utenti e computer di Active Directory.  
  
3.  Fare clic con il pulsante destro del mouse sull'unità organizzativa in cui sono presenti tutti gli oggetti utente per Cartelle di lavoro (se gli utenti sono archiviati in più unità organizzative o domini, fare clic con il pulsante destro del mouse sul contenitore comune a tutti gli utenti) e quindi scegliere **Delega controllo…** . Verrà visualizzata la Delega guidata del controllo.  
  
4.  Nella pagina **Utenti o gruppi** fai clic su **Aggiungi...** quindi specifica il gruppo creato per gli amministratori di Cartelle di lavoro (ad esempio, **Amministratori Cartelle di lavoro**).  
  
5.  Nella pagina **Attività da delegare** fare clic su **Crea un'attività personalizzata per eseguire la delega**.  
  
6.  Nella pagina **Tipo di oggetti Active Directory** fare clic su **Solo i seguenti oggetti contenuti nella cartella** e quindi selezionare la casella di controllo **Oggetti utente**.  
  
7.  Nella pagina **Autorizzazioni** deselezionare la casella di controllo **Generale**, selezionare la casella di controllo **Proprietà** e quindi selezionare le caselle di controllo **Lettura msDS-SyncServerUrl** e **Scrittura msDS-SyncServerUrl**.

Per delegare il permesso di modificare la proprietà msDS-SyncServerURL negli oggetti utente mediante Windows PowerShell, utilizzare lo script di esempio seguente che include il comando DsAcls.
  
```powershell  
$GroupName = "Contoso\Work Folders Administrators"  
$ADGroupPath = "CN=Users,dc=contoso,dc=com"  
  
DsAcls $ADGroupPath /I:S /G ""$GroupName":RPWP;msDS-SyncServerUrl;user"  
```  
  
> [!NOTE]
>  L'esecuzione dell'operazione di delega potrebbe richiedere tempo nei domini con un numero elevato di utenti.  
  
## <a name="step-7-create-sync-shares-for-user-data"></a>Passaggio 7: Creare condivisioni di sincronizzazione per i dati degli utenti  
 A questo punto, è possibile designare una cartella del server di sincronizzazione in cui archiviare i file degli utenti. Per creare questa cartella, denominata condivisione di sincronizzazione, è possibile utilizzare la procedura seguente.  
  
1. Se non disponi già di un volume NTFS con spazio libero per la condivisione di sincronizzazione e per i file degli utenti che conterrà, crea un nuovo volume e formattalo con il file system NTFS.  
  
2. In Server Manager fare clic su **Servizi file e archiviazione** e quindi su **Cartelle di lavoro**.  
  
3. Nella parte superiore del riquadro dei dettagli è disponibile un elenco delle condivisioni di sincronizzazione esistenti. Per creare una nuova condivisione di sincronizzazione, dal menu **Attività** scegliere **Nuova condivisione di sincronizzazione…** . Verrà visualizzata la Creazione guidata nuova condivisione di sincronizzazione.  
  
4. Nella pagina **Selezionare il server e il percorso**, specificare il percorso in cui archiviare la condivisione di sincronizzazione. Se è già disponibile una condivisione file per questi dati degli utenti, è possibile sceglierla. In alternativa, è possibile creare una nuova cartella.  
  
   > [!NOTE]
   >  Per impostazione predefinita, le condivisioni di sincronizzazione non sono direttamente accessibili tramite una condivisione file (a meno che non si selezioni una condivisione file esistente). Se si desidera rendere una condivisione di sincronizzazione disponibile tramite una condivisione file, utilizza il riquadro **Condivisioni** di Server Manager oppure il cmdlet [New-SmbShare](https://technet.microsoft.com/library/jj635722.aspx) per creare una condivisione file in cui sia possibilmente abilitata l'enumerazione basata sull'accesso.  
  
5. Nella pagina **Specificare la struttura delle cartelle utente** scegliere una convenzione di denominazione per le cartelle degli utenti incluse nella condivisione di sincronizzazione. Sono disponibili due opzioni:  
  
   - **Alias utente** consente di creare cartelle utente che non includano un nome di dominio. Se si utilizza una condivisione file già in uso con Reindirizzamento cartelle o un'altra soluzione per i dati degli utenti, selezionare questa convenzione di denominazione. È facoltativamente possibile selezionare la casella di controllo **Sincronizza solo la sottocartella seguente** per sincronizzare solo una sottocartella specifica, ad esempio la cartella Documenti.  
  
   - <strong>Utente alias@domain</strong> consente di creare cartelle degli utenti che includano un nome di dominio. Se non si utilizza una condivisione file già in uso con Reindirizzamento cartelle o un'altra soluzione per i dati degli utenti, seleziona questa convenzione di denominazione per evitare possibili conflitti di denominazione quando più utenti della condivisione hanno lo stesso alias (caso che può verificarsi se gli utenti appartengono a domini diversi).  
  
6. Nella pagina **Immettere il nome della condivisione di sincronizzazione** specificare un nome e una descrizione per la condivisione di sincronizzazione. Questi dati non vengono visualizzati in rete ma sono disponibili in Server Manager e Windows Powershell per distinguere le condivisioni di sincronizzazione tra loro.  
  
7. Nella pagina **Concedi accesso per sincronizzazione ai gruppi** specificare il gruppo creato in cui sono elencati gli utenti a cui è consentito utilizzare questa condivisione di sincronizzazione.  
  
   > [!IMPORTANT]
   >  Per migliorare le prestazioni e la sicurezza, è consigliabile consentire l'accesso a gruppi anziché a singoli utenti ed essere il più specifici possibile evitando gruppi generici come Authenticated Users e Domain Users. Se si consente l'accesso a gruppi con un numero elevato di utenti, aumenta il tempo impiegato da Cartelle di lavoro per eseguire la query in AD DS. In presenza di un numero elevato di utenti, è consigliabile creare più condivisioni di sincronizzazione per distribuire il carico.  
  
8. Nella pagina **Specifica criteri per i dispositivi** specificare se si desidera richiedere eventuali limitazioni di sicurezza nei computer e nei dispositivi client. Sono disponibili due criteri:  
  
   -   **Crittografa Cartelle di lavoro** consente di crittografare Cartelle di lavoro nei computer e nei dispositivi client  
  
   -   **Blocca automaticamente lo schermo e richiedi una password** consente di bloccare automaticamente gli schermi dei computer e dei dispositivi client dopo 15 minuti, di richiedere una password di almeno sei caratteri per sbloccare lo schermo e di attivare la modalità di blocco del dispositivo dopo 10 tentativi non riusciti  
  
       > [!IMPORTANT]
       >  Per applicare i criteri per le password per i computer Windows 7 e per gli utenti non amministratori nei computer aggiunti al dominio, usare i criteri per le password di Criteri di gruppo per i domini dei computer ed escludere questi domini dai criteri per le password di Cartelle di lavoro. È possibile escludere domini utilizzando il cmdlet [Set-Syncshare -PasswordAutoExcludeDomain](https://technet.microsoft.com/library/dn296649\(v=wps.630\).aspx) dopo la creazione della condivisione di sincronizzazione. Per informazioni sulla configurazione dei criteri per le password per Criteri di gruppo, vedere [Criteri per le password](https://technet.microsoft.com/library/hh994572(v=ws.11).aspx).  
  
9. Controllare le selezioni effettuate e completare la procedura guidata per creare la condivisione di sincronizzazione.

È possibile creare condivisioni di sincronizzazione mediante Windows PowerShell utilizzando il cmdlet [New-SyncShare](https://technet.microsoft.com/library/dn296635.aspx). Di seguito è disponibile un esempio di questo metodo:  
  
```powershell  
New-SyncShare "HR Sync Share" K:\Share-1 –User "HR Sync Share Users"  
```  
  
L'esempio precedente consente di creare una nuova condivisione di sincronizzazione denominata *Condivisione01* con percorso *K:\Condivisione-1*, accessibile per il gruppo denominato *Utenti condivisione sincronizzazione HR*  
  
> [!TIP]
>  Dopo aver creato le condivisioni di sincronizzazione, è possibile utilizzare la funzionalità Gestione risorse file server per gestire i dati nelle condivisioni. Ad esempio, è possibile utilizzare il riquadro **Quota** della pagina Cartelle di lavoro in Server Manager per impostare quote nelle cartelle degli utenti. È inoltre possibile utilizzare [Gestione screening dei file](https://technet.microsoft.com/library/cc732074.aspx) per controllare i tipi di file sincronizzati da Cartelle di lavoro oppure utilizzare gli scenari descritti in [Controllo dinamico degli accessi](https://technet.microsoft.com/windows-server-docs/identity/solution-guides/dynamic-access-control--scenario-overview) per attività di classificazione dei file più avanzate.  
  
## <a name="step-8-optionally-specify-a-tech-support-email-address"></a>Passaggio 8: Facoltativamente specificare un indirizzo di posta elettronica del supporto tecnico   
 Dopo l'installazione di Cartelle di lavoro in un file server, è consigliabile specificare un indirizzo di posta elettronica di un contatto amministrativo per il server. Per aggiungere un indirizzo di posta elettronica, utilizzare la procedura seguente:  
  
#### <a name="specifying-an-administrative-contact-email"></a>Specifica di un indirizzo di posta elettronica di un contatto amministrativo 
  
1.  In Server Manager fare clic su **Servizi file e archiviazione** e quindi su **Server**.  
  
2.  Fare clic con il pulsante destro del mouse sul server di sincronizzazione e quindi scegliere **Impostazioni di Cartelle di lavoro**. Verrà visualizzata la finestra Impostazioni di Cartelle di lavoro.  
  
3.  Nel riquadro di spostamento fare clic su **Posta elettronica del supporto tecnico** e quindi digitare l'indirizzo o gli indirizzi di posta elettronica che gli utenti possono utilizzare per richiedere assistenza relativa a Cartelle di lavoro. Al termine, fai clic su **OK**.  
  
     Nel Pannello di controllo di Cartelle di lavoro gli utenti di Cartelle di lavoro possono fare clic su un collegamento che consente di inviare un messaggio di posta elettronica contenente informazioni diagnostiche sul computer client all'indirizzo o agli indirizzi specificati qui.  
  
## <a name="step-9-optionally-set-up-server-automatic-discovery"></a>Passaggio 9: Configurare facoltativamente l'individuazione automatica del server  
 Se nell'ambiente in uso sono ospitati più server di sincronizzazione, è consigliabile configurare l'individuazione automatica del server popolando la proprietà **msDS-SyncServerURL** negli account utente in AD DS.  
  
>[!NOTE]
>La proprietà msDS-SyncServerURL in Active Directory non deve essere definita per gli utenti remoti che accedono a Cartelle di lavoro tramite una soluzione di proxy inverso, ad esempio Proxy applicazione Web o Proxy di applicazione di Azure AD. Se la proprietà msDS-SyncServerURL è definita, il client Cartelle di lavoro tenterà di accedere a un URL interno che non è accessibile tramite la soluzione di proxy inverso. Quando si utilizza Proxy applicazione Web o proxy dell'applicazione Azure AD, devi creare applicazioni proxy univoche per ogni server Cartelle di lavoro. Per altre informazioni, vedere [distribuzione di cartelle di lavoro con AD FS e Proxy applicazione Web: Cenni preliminari sulla](deploy-work-folders-adfs-overview.md) oppure [distribuzione di cartelle di lavoro con Azure AD Application Proxy](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/).


 Prima di poter eseguire questa operazione, è necessario installare un controller di dominio di Windows Server 2012 R2 oppure aggiornare gli schemi della foresta e del dominio utilizzando i comandi `Adprep /forestprep` e `Adprep /domainprep`. Per informazioni su come eseguire correttamente questi comandi, vedere [Eseguire Adprep](https://technet.microsoft.com/library/dd464018.aspx).  
  
 È inoltre possibile creare un gruppo di sicurezza per gli amministratori di file server e concedere loro autorizzazioni delegate per la modifica di questo attributo utente specifico, come illustrato nei passaggi 5 e 6. Se non si eseguono questi passaggi, un membro del gruppo Domain Admins o Enterprise Admins deve configurare l'individuazione automatica per ogni utente.  
  
#### <a name="to-specify-the-sync-server-for-users"></a>Per specificare il server di sincronizzazione per gli utenti  
  
1.  Aprire Server Manager in un computer in cui sono installati gli strumenti di amministrazione di Active Directory.  
  
2.  Dal menu **Strumenti** scegliere **Centro amministrativo di Active Directory**. Verrà visualizzato Centro amministrativo di Active Directory.  
  
3.  Passare al contenitore **Utenti** del dominio appropriato, fare clic con il pulsante destro del mouse sull'utente che si desidera assegnare a una condivisione di sincronizzazione e quindi scegliere **Proprietà**.  
  
4.  Nel riquadro di spostamento fare clic su **Estensioni**.  
  
5.  Fare clic sulla scheda **Editor attributi**, selezionare **msDS-SyncServerUrl** e quindi fare clic su **Modifica**. Verrà visualizzata la finestra di dialogo Editor stringa multivalore.  
  
6.  Nella casella **Valore da aggiungere** digitare l'URL del server di sincronizzazione con cui si desidera che l'utente esegua la sincronizzazione, fare clic su **Aggiungi**, su **OK** e quindi di nuovo su **OK**.  
  
    > [!NOTE]
    >  L'URL del server di sincronizzazione è semplicemente `https://` o `http://` (a seconda che si desideri utilizzare una connessione sicura) seguito dal nome di dominio completo del server di sincronizzazione. Ad esempio, **https:\//sync1.contoso.com**.

Per popolare l'attributo per più utenti, utilizzare Active Directory PowerShell. L'esempio seguente consente di popolare l'attributo per tutti i membri del gruppo *Utenti condivisione sincronizzazione HR* illustrato nel passaggio 5.
  
```powershell  
$SyncServerURL = "https://sync1.contoso.com"  
$GroupName = "HR Sync Share Users"  
  
Get-ADGroupMember -Identity $GroupName |  
Set-ADUser –Add @{"msDS-SyncServerURL"=$SyncServerURL}  
  
```  
  
## <a name="step-10-optionally-configure-web-application-proxy-azure-ad-application-proxy-or-another-reverse-proxy"></a>Passaggio 10: Se si desidera configurare Proxy applicazione Web, Azure AD Application Proxy o un altro proxy inverso  

Per consentire agli utenti remoti di accedere ai loro file, è necessario pubblicare il server Cartelle di lavoro tramite un proxy inverso per renderle disponibili esternamente su Internet. È possibile utilizzare Proxy applicazione Web, Proxy di applicazione di Azure Active Directory o un'altra soluzione di proxy inverso.  
  
Per configurare l'accesso a Cartelle di lavoro con AD FS e il proxy dell'applicazione Web, vedere [Distribuzione di Cartelle di lavoro con Active Directory Federation Services e Proxy applicazione Web](deploy-work-folders-adfs-overview.md). Per informazioni di carattere generale sul proxy dell'applicazione Web, vedi [Proxy dell'applicazione Web in Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server). Per ulteriori informazioni sulla pubblicazione di applicazioni, ad esempio Cartelle di lavoro su Internet mediante Proxy dell'applicazione Web, vedi [Pubblicazione delle applicazioni con la preautenticazione di AD FS](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/publishing-applications-using-ad-fs-preauthentication).
 
Per configurare l'accesso a Cartelle di lavoro tramite Proxy di applicazione di Azure Active Directory, vedere [Abilitare l'accesso remoto a Cartelle di lavoro con Proxy dell'applicazione Azure Active Directory](https://blogs.technet.microsoft.com/filecab/?p=7945) 
  
## <a name="step-11-optionally-use-group-policy-to-configure-domain-joined-pcs"></a>Passaggio 11: Usare facoltativamente Criteri di gruppo per configurare computer che fanno parte di un dominio  

Se si dispone di un numero elevato di computer che fanno parte di un dominio in cui si desidera distribuire Cartelle di lavoro, è possibile utilizzare Criteri di gruppo per eseguire le attività di configurazione seguenti nei computer client:  
  
- Specificare il server di sincronizzazione con cui gli utenti devono eseguire la sincronizzazione  
  
- Imporre la configurazione automatica di Cartelle di lavoro utilizzando le impostazioni predefinite. Prima di eseguire questa operazione, vedere la sezione relativa a Criteri di gruppo in [Progettazione di un'implementazione di Cartelle di lavoro](plan-work-folders.md).  
  
  Per controllare queste impostazioni, creare un nuovo oggetto Criteri di gruppo per Cartelle di lavoro e quindi configurare le impostazioni di Criteri di gruppo seguenti nel modo appropriato:  
  
- Criterio che consente di specificare le impostazioni di Cartelle di lavoro in Configurazione utente\Criteri\Modelli amministrativi\Componenti di Windows\WorkFolders  
  
- Criterio che consente di imporre la configurazione automatica per tutti gli utenti in Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\WorkFolders  
  
> [!NOTE]
>  Queste impostazioni dei criteri sono disponibili solo quando si modifica Criteri di gruppo da un computer che esegue Gestione Criteri di gruppo in Windows 8.1, Windows Server 2012 R2 o versioni successive. Nelle versioni di Gestione Criteri di gruppo dei sistemi operativi precedenti non è disponibile questa funzionalità. Queste impostazioni dei criteri si applicano ai computer con Windows 7 in cui l'app [Cartelle di lavoro per Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) è stata installata.  
  
##  <a name="BKMK_LINKS"></a> Vedere anche  
 Per altre informazioni correlate, vedere le risorse seguenti.  
  
|Tipo di contenuto|Riferimenti|  
|------------------|----------------|  
|**Informazioni su**|-   [Cartelle di lavoro](work-folders-overview.md)|  
|**Pianificazione**|-   [Progettazione di un'implementazione di cartelle di lavoro](plan-work-folders.md)|
|**Distribuzione**|-   [Distribuzione di cartelle di lavoro con AD FS e Proxy applicazione Web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [Distribuzione di Lab di Test di cartelle di lavoro](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (post di blog)<br />-   [Un nuovo attributo utente per l'Url del server di cartelle di lavoro](http://blogs.technet.com/b/filecab/archive/2013/10/09/a-new-user-attribute-for-work-folders-server-url.aspx) (post di blog)|  
|**Riferimento tecnico**|-   [Accesso interattivo: Soglia di blocco degli account computer](https://technet.microsoft.com/library/jj966264(v=ws.11).aspx)<br />-   [Cmdlet di condivisione di sincronizzazione](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)|
