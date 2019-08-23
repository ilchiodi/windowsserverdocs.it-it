---
title: Gestire Office 365 in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: bd7787b853395bf461165802251de8b2be5d5a39
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980262"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gestire Office 365 in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si integra il server di Windows Server Essentials con Microsoft Office 365, è possibile gestire i servizi e gli account online di Office 365 insieme alle risorse locali dal dashboard di Windows Server Essentials. In questo argomento si scoprirà cosa è possibile ottenere integrando il server con Office 365, come procedere e come gestire e risolvere i problemi relativi all'integrazione di Office 365.  
  
  
  
> [!IMPORTANT]
>   L'integrazione di Office 365 è supportata solo in un singolo ambiente di controller di dominio. Inoltre, l'integrazione guidata di Office 365 deve essere eseguita in un controller di dominio.  
  
## <a name="in-this-topic"></a>Contenuto dell'argomento  
  
-   [Perché integrare Office 365 con My Server?](#BKMK_IntegrationOverview)  
  
-   [Configurare l'integrazione di Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gestire l'integrazione con Office 365](#BKMK_ManageIntegration)  
  
-   [Risolvere i problemi di integrazione con Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a>Perché integrare Office 365 con My Server?  
 Esistono molti motivi validi per integrare Office 365 con il server Windows Server Essentials. Se si gestiscono alcune risorse internamente, ma si usa Office 365 per altri servizi, sarà possibile gestire i servizi e le risorse di Office 365 dal dashboard, insieme alle risorse locali, invece di lavorare in due posizioni.  
  
- Gestire gli account online che consentono agli utenti di accedere a Office 365 insieme agli account utente:  
  
  -   Creare gli account di Microsoft Online Services per gli utenti in un solo passaggio oppure creare gli account utente sul server per gli account online esistenti. È anche possibile aggiungere un account online a un account utente nuovo o esistente.  
  
       Quando si creano gli account online dal dashboard, gli utenti accedono a Office 365 con la stessa password usata sul server. Se cambiano la password per l'account utente, cambia anche la password online. In questo modo si è certi che le password degli account online rispetteranno sempre i requisiti di sicurezza impostati per gli account utente.  
  
  -   Gestire un account online insieme all'account utente per tutto il ciclo di vita dell'account utente. Se si disattiva un account utente, viene disattivato anche l'account online. Se si rimuove un account utente, viene rimosso anche l'account online.  
  
  -   In un server di Windows Server Essentials, è possibile gestire anche i gruppi di distribuzione di Exchange Online per la posta elettronica.  
  
- Inviare e ricevere messaggi di posta elettronica dal dominio Internet dell'organizzazione (ad esempio, contoso.com) collegando un dominio Internet personalizzato alla sottoscrizione di Office 365.  
  
- Gestire la sottoscrizione e l'integrazione di Office 365 dal dashboard.  
  
- Se la sottoscrizione include le raccolte di SharePoint Online, l'integrazione di Office 365 con un server di Windows Server Essentials consente di:  
  
  - Creare e gestire le raccolte di SharePoint Online dal dashboard.  
  
    > [!NOTE]
    >  Sarà anche possibile usare l'app My Server 2012 R2 per lavorare con i documenti nelle raccolte di SharePoint Online dal portatile, dal dispositivo mobile o da Windows Phone. Questa funzionalità è disponibile solo per Windows Server Essentials. Per altre informazioni, vedere [usare l'app My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
  - Modificare le autorizzazioni per un sito del team di SharePoint Online dal dashboard o aprire il sito del team dal dashboard per apportare altre modifiche.  
  
    Per altre informazioni, vedere [gestire SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
- Se si sottoscrive Exchange Online, l'integrazione di Office 365 con un server di Windows Server Essentials consente di gestire i dispositivi mobili usati dagli utenti per connettersi al server di posta elettronica aziendale:  
  
  -   Richiedere la password di protezione quando i dispositivi mobili si connettono al server di posta elettronica aziendale. Impostare una lunghezza minima per la password, il numero di tentativi di accesso non riusciti consentiti e il periodo di tempo minimo che deve trascorrere tra due tentativi di accesso.  
  
  -   Impedire a un dispositivo mobile di connettersi a Exchange Online se si è conoscenza di problemi di sicurezza con il modello del dispositivo.  
  
  -   Se un dispositivo mobile viene perso o rubato, cancellare i dati nel dispositivo per eliminare i dati sensibili della società la prossima volta che il dispositivo viene acceso.  
  
- L'integrazione di Office 365 offre nuovi modi per connettersi ai servizi e alle risorse di Office 365:  
  
  -   Aprire i servizi di Office 365 dalla finestra di avvio di Windows Server Essentials. Per informazioni, vedere [Guida introduttiva all'uso di Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
  -   Usare l'app My Server 2012 R2 per lavorare con i documenti nelle raccolte di SharePoint Online dal portatile, dal dispositivo mobile o da Windows Phone. Per informazioni, vedere [usare l'app My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Questa funzionalità è disponibile solo in Windows Server Essentials.  
  
##  <a name="BKMK_Configure"></a>Configurare l'integrazione di Office 365  
 È possibile integrare il server con Office 365 in qualsiasi momento dopo aver completato l'installazione del server. Se non si ha già una sottoscrizione di Office 365, è possibile acquistarne una o registrarsi per una sottoscrizione di valutazione gratuita.  
  
 Si eseguiranno le attività seguenti:  
  
-   [Passaggio 1: Verificare i requisiti di integrazione di Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Passaggio 2: Integrare il server con Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Passaggio 3: Collegare il nome di dominio Internet dell'organizzazione a Office 365 (facoltativo)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a>Passaggio 1: Verificare i requisiti di integrazione di Office 365  
 Prima di iniziare, verificare che il server soddisfi questi requisiti:  
  
-   Il server può avere uno di questi sistemi operativi:  Windows Server Essentials, Windows Server Essentials o il sistema operativo Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato.  
  
-   L'ambiente può avere un solo controller di dominio ed è necessario eseguire l'integrazione di Office 365 sul controller di dominio.  
  
-   È necessario potersi connettere a Internet dal server.  
  
-   È necessario installare tutti gli aggiornamenti critici e importanti nel server prima di avviare l'integrazione di Office 365.  
  
-   Se si vuole usare il dominio Internet dell'organizzazione negli indirizzi di posta elettronica e con le risorse di SharePoint Online, è necessario registrare il nome di dominio in modo da poter collegare il dominio a Office 365 durante l'integrazione. Per altre informazioni, vedere [Come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Non è necessario sottoscrivere Office 365 in anticipo. Potrai acquistare una sottoscrizione o iscriverti per ottenere una versione di valutazione gratuita durante l'integrazione con Office 365. Per esaminare i piani e i prezzi di Office 365, [confrontare i piani di office 365 per le aziende](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a>Passaggio 2: Integrare il server con Microsoft Office 365  
 Eseguire la procedura seguente nel controller di dominio per integrare il server di Windows Server Essentials con Office 365.  
  
> [!NOTE]
>  La procedura è identica, indipendentemente dal fatto che si disponga di Windows Server Essentials o Windows Server Essentials, ma si inizierà da una posizione diversa nella Home page. In Windows Server Essentials, è possibile integrare il server con Office 365 e altri Microsoft Online Services nella scheda **Servizi** . In Windows Server Essentials, l'integrazione di Office 365 viene eseguita nella scheda **posta elettronica** .  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Per integrare il server con Office 365  
  
1. Accedere al server come amministratore e aprire il dashboard di Windows Server Essentials.  
  
2. Nella **Home** page fare clic su **Servizi** (in Windows Server Essentials, fare clic su **posta elettronica**), fare clic su **integra con Microsoft Office 365**, quindi fare clic su **configura Microsoft Office l'integrazione con 365**.  
  
    Viene visualizzata l'integrazione guidata con Microsoft Office 365.  
  
3. Nella pagina **Attività iniziali** eseguire una delle azioni seguenti:  
  
   -   Se non si ha una sottoscrizione a Office 365, fare clic su **Avanti**e seguire le istruzioni per sottoscrivere Office 365 o iscriversi per una sottoscrizione di valutazione.  
  
        È necessario accedere a Office 365 prima di tornare alla procedura guidata. Tuttavia, non è necessario eseguire alcuna delle attività nella sezione **inizia qui** del portale di Office 365.  
  
   -   Se si dispone già di una sottoscrizione a Office 365 che si desidera integrare con il server, selezionare si **dispone già di una sottoscrizione per office 365**, quindi fare clic su **Avanti**.  
  
4. Seguire le istruzioni per completare la procedura guidata.  
  
   Una volta completata la procedura guidata, si noteranno le seguenti modifiche al Dashboard:  
  
-   È disponibile una nuova pagina di **office 365** , che consente di gestire l'integrazione e la sottoscrizione di Office 365.  
  
-   Nella pagina **utenti** verrà visualizzata una scheda **account online** in cui è possibile creare e gestire gli account di Microsoft Online Services che consentono agli utenti di accedere a Office 365. Se si usa Exchange Online e si ha un server di Windows Server Essentials, verrà visualizzata anche una scheda **gruppi di distribuzione** .  
  
-   La pagina **archiviazione** in un server Windows Server Essentials include una scheda **raccolte SharePoint** per la gestione delle raccolte di SharePoint Online e la modifica delle autorizzazioni per i siti del team. Ogni piano aziendale per Office 365 include le funzionalità di base di SharePoint Online.  
  
###  <a name="BKMK_StepThree"></a>Passaggio 3: Collegare il nome di dominio Internet dell'organizzazione a Office 365 (facoltativo)  
 Se si vuole usare il proprio dominio Internet nella posta elettronica indirizzata all'organizzazione e gli URL per le risorse di SharePoint Online, è possibile collegare un dominio personalizzato alla sottoscrizione di Office 365. Se si integra il server Windows Server Essentials con Office 365, è possibile eseguire questa operazione dal dashboard.  
  
 È più semplice eseguire questa operazione prima di creare account online per gli utenti, in modo da poter usare il dominio quando si creano in blocco gli account online.  
  
 Quando si esegue l'iscrizione a Office 365, si ottiene un nome di dominio, ad esempio *Contoso*. onmicrosoft.com. Se si preferisce usare un nome di dominio diverso, è sufficiente contoso.com? è possibile. È necessario acquistare un nome di dominio se non ne è già proprietario uno e cambiare alcuni record DNS.  
  
 La configurazione di un dominio personalizzato da usare con Office 365 prevede quattro attività:  
  
1. **Acquistare un nome di dominio,** ovvero registrarlo con un registrar o un provider di hosting DNS.  
  
   -   Selezionare un nome di dominio compatibile con Office 365. È possibile usare un nome di dominio di secondo livello? ad esempio, buycontoso.com? ma non un nome di dominio di terzo livello? ad esempio, marketing.contoso.com. Per altre informazioni sulla scelta di un dominio da usare in Office 365, vedere [domini](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
   -   Acquistarlo da un registrar che consente i record DNS (Domain Name Server) richiesti da Office 365. Per conoscere i registrar che consentono i record DNS richiesti, vedere [Come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Se il dominio è già stato registrato con un altro registrar, non è preoccupato; Quando si collega il dominio a Office 365, è possibile trasferire il dominio a un registrar diverso.  
  
2. **Configurare record DNS che consentano ai servizi di Office 365 di usare il nome di dominio.** Il modo più semplice consiste nel consentire alla procedura guidata di configurare automaticamente i record DNS quando il dominio viene collegato alla sottoscrizione di Office 365 nel passaggio 3. Se lo si preferisce, vedere [come configurare manualmente i record DNS per l'integrazione con Office 365](#BKMK_ManuallyConfigureDNS).  
  
3. **Collegare il dominio Internet personalizzato alla sottoscrizione di Office 365.** Si userà l'azione **collega un dominio a Office 365** .  
  
4. **Verificare che i servizi di Office 365 utilizzino il nuovo nome di dominio.**  
  
   Se si completano i passaggi 1 e 2 prima di usare l'attività **collega un dominio a office 365** , la procedura guidata può collegare il nome di dominio a Office 365. In alternativa, è possibile ricorrere a una procedura guidata per alcuni o tutti i passaggi 1 e 2.  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>Per collegare il dominio Internet dell'organizzazione a Office 365  
  
1.  Nel dashboard aprire la pagina **Office 365** e fare clic su **Collega un dominio a Office 365**.  
  
2.  Seguire le istruzioni per completare la procedura guidata.  
  
     La procedura guidata può essere utile per alcuni o tutti i passaggi per la registrazione, la configurazione e il collegamento di un nome di dominio Internet nuovo o esistente da usare in Office 365.  
  
     Fare clic sul collegamento alla Guida nella pagina della procedura guidata per ottenere le informazione necessarie per completare un'attività In alternativa, vedere la sezione configurare un nome di dominio di [gestisci accesso Web remoto in Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) per una panoramica del processo e i requisiti.  
  
    > [!NOTE]
    >  Per usare la procedura guidata per registrare un nuovo nome di dominio, è necessario ricorrere a uno dei provider di servizi dei nomi di dominio che collaborano con Microsoft per offrire una facile integrazione con la procedura guidata. Per trovare un registrar di nomi di dominio, vedere [Come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Se la procedura guidata rileva che il nome di dominio non è gestito dal server, sarà necessario configurare manualmente i record DNS necessari per completare la configurazione. Per le istruzioni, vedere [Come configurare manualmente i record DNS per l'integrazione con Office 365](#BKMK_ManuallyConfigureDNS), più avanti in questo argomento.  
  
4.  Verificare che il dominio sia in uso in Office 365.  
  
     Una volta completata la procedura guidata, il registrar verifica i record DNS. Questa operazione viene eseguita automaticamente. non è necessario eseguire alcuna operazione. Ma in genere richiede circa un'ora e talvolta un po' più a lungo. Al termine della verifica del dominio, nella pagina **Office 365** sarà presente un elenco del dominio dell'organizzazione.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a>Come configurare manualmente i record DNS per l'integrazione con Office 365  
 Se la procedura guidata Collega il dominio a Office 365 rileva che il nome di dominio non è gestito dal server, per completare la configurazione, è necessario configurare manualmente i record DNS (Domain Name Server) necessari. In tal caso, è presente un elenco di record DNS che è necessario configurare in **%username%\NewDNSRecords_ (n). txt**, dove *(n)* è un numero casuale.  
  
 La tabella seguente descrive i record DNS che è necessario aggiungere. I metodi di immissione possono variare a seconda del registrar di nomi di dominio. In caso di domande, rivolgersi al registrar di nomi di dominio per assistenza.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Record DNS necessari per collegare un nome di dominio Internet personalizzato a Office 365  
  
|Service|Record DNS necessari|Scopo|  
|-------------|--------------------------|-------------|  
|(Più servizi)|MX| Office 365 utilizza questo record per verificare che si sia proprietari di un nome di dominio specifico. Questo record MX non interferisce con il routing dei messaggi di posta elettronica.|  
|Exchange Online|MX|Fornisce il routing dei messaggi di posta elettronica. **Importante:**  Se si deve eseguire la migrazione della posta elettronica, non assegnare una preferenza zero (**0**) al nuovo record MX. Assicurarsi che il valore del record sia maggiore del valore assegnato al record MX corrente. Quando la migrazione della posta elettronica è completa e si è pronti per modificare il server di posta elettronica in Office 365, chiedere al registrar di nomi di dominio di reimpostare il valore di preferenza del nuovo record MX.|  
|Exchange Online|Alias (CNAME)|Record di individuazione automatica usato per aiutare gli utenti a configurare facilmente una connessione tra Exchange Online e un client di posta elettronica mobile o il client desktop Outlook. **Nota:**  Se si preferisce accedere a Outlook accesso Web usando il nome di dominio dell'organizzazione (ad esempio, http://mail.contoso.com) anziché l'URL standard (https://outlook.com/owa/office365.com), è possibile configurare il record alias (CNAME) come indicato di seguito: **Tipo = CNAME, TTL = 01:00:00, nome host = posta, indirizzo = mail. office365. com**|  
|Exchange Online|TXT|Specifica che outlook.com, il dominio usato dai server di posta elettronica di Office 365, è autorizzato a inviare messaggi di posta elettronica per conto del dominio. Creare questo record per evitare che la posta elettronica in uscita venga contrassegnata come posta indesiderata.|  
|Lync Online|SRV|Aiuta ad abilitare la federazione con altri servizi di messaggistica istantanea come Windows Live o Yahoo!.|  
|Lync Online|SRV|Record di individuazione automatica usato per aiutare gli utenti a configurare facilmente una connessione tra il client desktop Lync e Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Al termine della verifica del dominio, non tentare di aggiungere o apportare altre modifiche ai record DNS dal portale di Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a>Passaggio successivo  
  
-   Creare account di Microsoft Online Services per gli utenti  
  
     Per usare i servizi di Office 365, gli utenti devono avere un account di Microsoft Online Services associato all'account utente di rete. Con il dashboard è semplice. Se si usa una nuova sottoscrizione di Office 365, è possibile creare in blocco gli account online per gli account utente esistenti. Se si sta integrando un nuovo server con una sottoscrizione di Office 365 già in uso, è possibile importare gli account utente dagli account online esistenti. Per le procedure, vedere [gestire gli account online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  Nel dashboard di Windows Server Essentials gli account di Microsoft Online Services sono indicati come account di Office 365. È cambiata solo la terminologia, mentre gli account sono rimasti invariati.  
  
##  <a name="BKMK_ManageIntegration"></a>Gestire l'integrazione con Office 365  
 Dopo aver integrato il server con Office 365, nella pagina **office 365** del dashboard vengono visualizzate le informazioni relative alla sottoscrizione di Office 365, che rende disponibili le seguenti attività:  
  
-   [Gestire la sottoscrizione di Office 365](#BKMK_ManageO365) ? Modificare l'account amministratore usato per gestire la sottoscrizione. Aprire il dashboard di amministrazione di Office 365 per gestire la sottoscrizione.  
  
-   [Collegare il dominio Internet dell'organizzazione a Office 365](#BKMK_StepThree) ? Se si desidera essere in grado di inviare e ricevere posta elettronica indirizzata al proprio dominio, è possibile collegare il dominio a Office 365. (Descritto in precedenza in [passaggio 3: Collegare il dominio dell'organizzazione a Office 365](#BKMK_StepThree).)  
  
-   [Disabilitare l'integrazione di Office 365](#BKMK_Disable) ? Se non si vogliono gestire i servizi, la sottoscrizione e gli account online di Office 365 dal dashboard, è possibile disabilitare l'integrazione di Office 365. I servizi sono ancora disponibili nel portale di Office 365.  
  
###  <a name="BKMK_ManageO365"></a>Gestire la sottoscrizione di Office 365  
 Se è necessario apportare modifiche alla sottoscrizione di Office 365 mentre si lavora sul server, è possibile aprire la sottoscrizione in Office 365 dalla pagina **office 365** del dashboard. È anche possibile modificare l'account amministratore usato dal server per apportare modifiche ai servizi di Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Per aprire l'abbonamento nel dashboard di amministrazione di Office 365  
  
1.  Nel dashboard di Windows Server Essentials aprire la pagina **Office 365** .  
  
2.  In **Attività di configurazione**fare clic su **Gestione di Office 365**.  
  
3.  Accedere a Office 365 con l'account online Microsoft usato per gestire la sottoscrizione.  
  
     Si apre il dashboard di amministrazione di Office 365.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Per cambiare l'account amministratore di Office 365  
  
1.  Nel dashboard fare clic su **Office 365**.  
  
2.  In **Attività di configurazione**fare clic su **Modifica l'account dell'amministratore di Office 365**. Viene visualizzata la procedura guidata Cambia account amministratore. (In Windows Server Essentials, la procedura guidata è denominata configura l'account amministratore di Office 365).  
  
3.  Digitare le credenziali per l'account che si vuole usare per connettersi alla sottoscrizione di Office 365, quindi fare clic su **Avanti**.  
  
4.  Fare clic su **Chiudi**. Il dashboard viene riavviato.  
  
###  <a name="BKMK_Disable"></a>Disabilitare l'integrazione di Office 365  
 Se si decide di non gestire i servizi e gli account online di Office 365 dal dashboard, è possibile disabilitare l'integrazione di Office 365. L'abbonamento a Office 365 rimane attivo e le modifiche alla configurazione apportate dal Dashboard restano valide. Ad esempio, si riceverà un messaggio di posta elettronica destinato a un nome di dominio collegato alla sottoscrizione di Office 365. Non si perderà alcun messaggio di posta elettronica e i controlli impostati per i dispositivi mobili continueranno a essere usati per Exchange Online.  
  
 In futuro, si gestiranno la sottoscrizione, i servizi e le risorse di Office 365 in Office 365 e gli utenti dovranno gestire le password per gli account online in Office 365. La sincronizzazione delle password non viene più eseguita e la disabilitazione o la rimozione di un account utente non avrà alcun effetto sull'account online dell'utente.  
  
 Poiché il software di integrazione di Office 365 è installato nel server locale, è possibile disabilitare la funzionalità anche se il servizio di integrazione non è in grado di connettersi a Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Per disabilitare l'integrazione di Office 365 con il server  
  
1.  Nel dashboard fare clic su **Office 365**.  
  
2.  Fare clic su **Disabilita l'integrazione di Office 365**. Viene visualizzata la disabilitazione guidata di Office 365.  
  
3.  Seguire le istruzioni per completare la procedura guidata.  
  
> [!NOTE]
>  Per abilitare nuovamente l'integrazione con Office 365, usare l'attività **integra con office 365** nella scheda **servizio** della **Home** page del dashboard. Per istruzioni, vedere [passaggio 2: Integrare il server Windows Server Essentials con Microsoft Office 365](#BKMK_StepTwo), più indietro in questo argomento.  
  
##  <a name="BKMK_Troubleshoot"></a>Risolvere i problemi di integrazione con Office 365  
 Questa sezione fornisce informazioni che consentono di risolvere i problemi comuni che possono verificarsi quando si usano le funzionalità di integrazione di Office 365 in Windows Server Essentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a>Alcuni account di Microsoft Online Services non sono stati creati  
 **Descrizione**  
  
 Il tentativo di creare uno o più account di Microsoft Online Services dal dashboard non è riuscito.  
  
 **Soluzione**  
  
1.  Fare clic sul collegamento nella pagina di completamento della procedura guidata per aprire un file di risultati contenente informazioni dettagliate su ogni richiesta di creazione account non completata. Ad esempio, un risultato potrebbe informare che esiste già un account di Microsoft Online Services con il nome di un account richiesto.  
  
2.  Eseguire le azioni consigliate per risolvere ogni errore.  
  
3.  Se il problema persiste, riavviare il server e quindi provare nuovamente a create gli account online.  
  
###  <a name="BKMK_ProblemUninstalling"></a>Si è verificato un problema durante la disinstallazione dell'integrazione di Office 365  
 **Descrizione**  
  
 Si è verificato un errore sconosciuto durante il tentativo di disabilitare l'integrazione di Office 365.  
  
 **Soluzione**  
  
1.  Verificare che il computer sia connesso a Internet e quindi riprovare.  
  
2.  Se l'errore si verifica ancora, riavviare il server e quindi riprovare.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica dell'integrazione dei servizi per Windows Server Essentials-parte 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Panoramica dell'integrazione dei servizi per Windows Server Essentials-parte 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guida introduttiva all'uso di Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gestire i Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
