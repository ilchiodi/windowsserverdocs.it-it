---
title: Gestire Office 365 in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
0author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d897c9186ff74a80d531cf84961709de54459f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433283"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gestire Office 365 in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si integra il server Windows Server Essentials con Microsoft Office 365, è possibile gestire i servizi di Office 365 e gli account online insieme alle risorse locali dal Dashboard di Windows Server Essentials. In questo argomento, scoprirai i vantaggi grazie all'integrazione del server con Office 365, eseguire la procedura e come gestire e risolvere i problemi di integrazione di Office 365.  
  
  
  
> [!IMPORTANT]
>   Integrazione di Office 365 è supportata solo in un ambiente di controller di dominio singolo. È inoltre necessario eseguire la procedura guidata integrazione di Office 365 in un controller di dominio.  
  
## <a name="in-this-topic"></a>Contenuto dell'argomento  
  
-   [Perché integrare Office 365 con il mio server?](#BKMK_IntegrationOverview)  
  
-   [Configurare l'integrazione di Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gestire l'integrazione di Office 365](#BKMK_ManageIntegration)  
  
-   [Risolvere i problemi di integrazione di Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a> Perché integrare Office 365 con il mio server?  
 Esistono numerosi motivi validi per integrare Office 365 con il server di Windows Server Essentials. Se si gestiscono alcune risorse internamente, ma usare Office 365 per gli altri servizi, sarà in grado di gestire i servizi di Office 365 e le risorse dal Dashboard, insieme alle risorse locali, invece di lavorare in due posizioni.  
  
- Gestire gli account online che concedono agli utenti l'accesso a Office 365 insieme agli account utente:  
  
  -   Creare gli account di Microsoft Online Services per gli utenti in un solo passaggio oppure creare gli account utente sul server per gli account online esistenti. È anche possibile aggiungere un account online a un account utente nuovo o esistente.  
  
       Quando si creano gli account online dal Dashboard, gli utenti accedono a Office 365 con la stessa password usata sul server. Se cambiano la password per l'account utente, cambia anche la password online. In questo modo si è certi che le password degli account online rispetteranno sempre i requisiti di sicurezza impostati per gli account utente.  
  
  -   Gestire un account online insieme all'account utente per tutto il ciclo di vita dell'account utente. Se si disattiva un account utente, viene disattivato anche l'account online. Se si rimuove un account utente, viene rimosso anche l'account online.  
  
  -   In un server di Windows Server Essentials, gestire anche i gruppi di distribuzione di Exchange Online per la posta elettronica.  
  
- Inviare e ricevere posta elettronica dal dominio Internet dell'organizzazione (ad esempio, contoso.com) collegando un dominio Internet personalizzato all'abbonamento a Office 365.  
  
- Gestire la sottoscrizione e l'integrazione di Office 365 dal Dashboard.  
  
- Se la sottoscrizione include raccolte di SharePoint Online, integrazione di Office 365 con un server Windows Server Essentials consente di:  
  
  - Creare e gestire raccolte di SharePoint Online dal Dashboard.  
  
    > [!NOTE]
    >  È anche possibile usare l'app My Server 2012 R2 per lavorare con i documenti nelle raccolte di SharePoint Online dal portatile, dei dispositivi mobili o telefono Windows. Questa funzionalità è disponibile solo per Windows Server Essentials. Per altre informazioni, vedere [usare l'App My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
  - Modificare le autorizzazioni per un sito del team SharePoint Online dal Dashboard o aprire il sito del team dal Dashboard per apportare altre modifiche.  
  
    Per altre informazioni, vedere [gestire SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
- Se si sottoscrive Exchange Online, integrazione di Office 365 con un server Windows Server Essentials consente di gestire i dispositivi mobili che gli utenti usano per connettersi al server di posta elettronica aziendale:  
  
  -   Richiedere la password di protezione quando i dispositivi mobili si connettono al server di posta elettronica aziendale. Impostare una lunghezza minima per la password, il numero di tentativi di accesso non riusciti consentiti e il periodo di tempo minimo che deve trascorrere tra due tentativi di accesso.  
  
  -   Impedire a un dispositivo mobile di connettersi a Exchange Online se si è conoscenza di problemi di sicurezza con il modello del dispositivo.  
  
  -   Se un dispositivo mobile viene perso o rubato, cancellare i dati nel dispositivo per eliminare i dati sensibili della società la prossima volta che il dispositivo viene acceso.  
  
- Integrazione di Office 365 offre nuovi modi per connettersi alle risorse e servizi di Office 365:  
  
  -   Aprire Servizi di Office 365 dalla finestra di avvio di Windows Server Essentials. Per informazioni, vedere [Quick Start Guide to Using Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
  -   Usare l'app My Server 2012 R2 per lavorare con i documenti nelle raccolte di SharePoint Online dal portatile, dei dispositivi mobili o telefono Windows. Per informazioni, vedere [usare l'App My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Questa funzionalità è disponibile solo in Windows Server Essentials.  
  
##  <a name="BKMK_Configure"></a> Configurare l'integrazione di Office 365  
 È possibile integrare il server con Office 365 in qualsiasi momento dopo aver completato l'installazione del server. Se non si ha già una sottoscrizione Office 365, è possibile acquistarne uno o l'iscrizione per una sottoscrizione di valutazione gratuita.  
  
 Si eseguiranno le attività seguenti:  
  
-   [Passaggio 1: Verificare i requisiti di integrazione di Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Passaggio 2: Integrare il server con Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Passaggio 3: Collegare il nome di dominio Internet dell'organizzazione a Office 365 (facoltativo)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a> Passaggio 1: Verificare i requisiti di integrazione di Office 365  
 Prima di iniziare, verificare che il server soddisfi questi requisiti:  
  
-   Il server può avere uno di questi sistemi operativi:  Windows Server Essentials, Windows Server Essentials o il sistema operativo Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato.  
  
-   L'ambiente può avere solo un controller di dominio ed è necessario eseguire l'integrazione di Office 365 nel controller di dominio.  
  
-   È necessario potersi connettere a Internet dal server.  
  
-   Prima di iniziare l'integrazione di Office 365, è necessario installare tutti gli aggiornamenti critici e importanti nel server.  
  
-   Se si desidera usare il dominio Internet dell'organizzazione negli indirizzi di posta elettronica e con le risorse di SharePoint Online, è necessario registrare il nome di dominio in modo che è possibile collegare il dominio a Office 365 durante l'integrazione. Per altre informazioni, vedere [Come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Non devi eseguire la sottoscrizione a Office 365 in anticipo. Sarà in grado di acquistare un abbonamento o iscriversi per una versione di valutazione gratuita durante l'integrazione di Office 365. Se si desidera esaminare i piani e prezzi per Office 365 [confrontare i piani di Office 365 per aziende](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a> Passaggio 2: Integrare il server con Microsoft Office 365  
 Eseguire la procedura seguente nel controller di dominio per l'integrazione del server Windows Server Essentials con Office 365.  
  
> [!NOTE]
>  La procedura 's lo stesso se si dispone di Windows Server Essentials o Windows Server Essentials, ma si inizierà da una posizione diversa nella Home page. In Windows Server Essentials, si integra il server con Office 365 e altri Microsoft Online Services nel **Services** scheda. In Windows Server Essentials, integrazione di Office 365 verrà eseguita nel **messaggio di posta elettronica** scheda.  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Per integrare il server con Office 365  
  
1. Accedere al server come amministratore e aprire il Dashboard di Windows Server Essentials.  
  
2. Nel **Home** pagina, fare clic su **Services** (in Windows Server Essentials, fare clic su **posta elettronica**), fare clic su **integra con Microsoft Office 365**, e quindi fare clic su **configurare l'integrazione di Microsoft Office 365**.  
  
    Viene visualizzata l'integrazione guidata con Microsoft Office 365.  
  
3. Nella pagina **Attività iniziali** eseguire una delle azioni seguenti:  
  
   -   Se non hai una sottoscrizione a Office 365, fare clic su **successivo**e seguire le istruzioni per la sottoscrizione a Office 365 o segno di backup di una sottoscrizione di valutazione.  
  
        È necessario accedere a Office 365 prima di tornare alla procedura guidata. Ma non è necessario eseguire nessuna delle attività dei **iniziare da qui** sezione del portale di Office 365.  
  
   -   Se hai già un abbonamento a Office 365 che si vuole integrare con il server, selezionare **se ha già una sottoscrizione di Office 365**, quindi fare clic su **successivo**.  
  
4. Seguire le istruzioni per completare la procedura guidata.  
  
   Dopo aver completato la procedura guidata, si noteranno le modifiche seguenti al Dashboard:  
  
-   C'è un nuovo **Office 365** pagina, che viene usato per gestire l'integrazione e la sottoscrizione di Office 365.  
  
-   Nel **gli utenti** pagina, si noterà un' **gli account Online** scheda in cui è possibile creare e gestire gli account Microsoft Online Services che danno agli utenti l'accesso a Office 365. Se si usa Exchange Online e si dispone di un server Windows Server Essentials, si noterà anche un **gruppi di distribuzione** scheda.  
  
-   Il **Storage** pagina in un server Windows Server Essentials include una **raccolte di SharePoint** scheda per la gestione di raccolte di SharePoint Online e modifica delle autorizzazioni per i siti del team. Ogni piano aziendale per Office 365 include queste funzionalità di SharePoint Online.  
  
###  <a name="BKMK_StepThree"></a> Passaggio 3: Collegare il nome di dominio Internet dell'organizzazione a Office 365 (facoltativo)  
 Se si desidera usare il proprio dominio Internet nella posta elettronica indirizzata all'organizzazione e gli URL delle risorse di SharePoint Online, è possibile collegare un dominio personalizzato all'abbonamento a Office 365. Se si integra il server di Windows Server Essentials con Office 365, è possibile eseguire questa operazione dal Dashboard.  
  
 È più semplice eseguire questa operazione prima di creare account online per gli utenti in modo che è possibile usare il dominio quando si creano in blocco gli account online.  
  
 Si ottiene un nome di dominio quando effettua l'iscrizione per Office 365? ad esempio, *contoso*. onmicrosoft.com. Se si preferisce usare un nome di dominio diversi? ad esempio semplicemente contoso.com? è possibile. È necessario acquistare un nome di dominio se non già possedere uno e cambiare alcuni record DNS.  
  
 Configurazione di un dominio personalizzato da usare con Office 365 include quattro attività:  
  
1. **Acquistare un nome di dominio,** ovvero registrarlo con un registrar o un provider di hosting DNS.  
  
   -   Scegliere un nome di dominio che funziona con Office 365. È possibile usare un nome di dominio di livello 2 °? ad esempio buycontoso.com? ma non un nome di dominio di livello 3 °? ad esempio marketing.contoso.com. Per altre informazioni sulla scelta di un dominio da usare in Office 365, vedere [domini](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
   -   Acquistarlo da un registrar che consente i record di server (DNS) nome di dominio richiesti da Office 365. Per conoscere i registrar che consentono i record DNS richiesti, vedere [Come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Non preoccuparsi se il dominio già registrato con un altro registrar, è possibile trasferire il dominio a altro registrar quando si collega il dominio a Office 365.  
  
2. **Configurare record DNS che consentano ai servizi di Office 365 di usare il nome di dominio.** Il modo più semplice è configurare automaticamente la procedura guidata i record DNS per l'utente quando si collega il dominio alla sottoscrizione di Office 365 nel passaggio 3. Se si preferisce eseguire l'operazione autonomamente, vedere [come configurare manualmente il DNS i record per l'integrazione di Office 365](#BKMK_ManuallyConfigureDNS).  
  
3. **Collegare il dominio Internet personalizzato all'abbonamento a Office 365.** Si userà il **collegare un dominio a Office 365** azione.  
  
4. **Verificare che i servizi di Office 365 usino il nuovo nome di dominio.**  
  
   Se si completano i passaggi 1 e 2 prima di usare la **collegare un dominio a Office 365** attività, la procedura guidata può collegare il nome di dominio a Office 365. In alternativa, è possibile ricorrere a una procedura guidata per alcuni o tutti i passaggi 1 e 2.  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>Il collegamento di dominio Internet dell'organizzazione a Office 365  
  
1.  Nel dashboard aprire la pagina **Office 365** e fare clic su **Collega un dominio a Office 365**.  
  
2.  Seguire le istruzioni per completare la procedura guidata.  
  
     La procedura guidata può aiutarti con alcuni o tutti i passaggi per la registrazione, configurazione e collegamento di un nome di dominio Internet nuovo o esistente da utilizzare in Office 365.  
  
     Fare clic sul collegamento alla Guida nella pagina della procedura guidata per ottenere le informazione necessarie per completare un'attività O visualizzare il Set di backup di una sezione Nome dominio [gestire accesso Web remoto in Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) per una panoramica del processo e requisiti.  
  
    > [!NOTE]
    >  Per usare la procedura guidata per registrare un nuovo nome di dominio, è necessario ricorrere a uno dei provider di servizi dei nomi di dominio che collaborano con Microsoft per offrire una facile integrazione con la procedura guidata. Per trovare un registrar di nomi di dominio, vedere [Come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Se la procedura guidata rileva che il nome di dominio non è gestito dal server, è necessario configurare manualmente i record DNS necessari per completare la configurazione. Per le istruzioni, vedere [Come configurare manualmente i record DNS per l'integrazione con Office 365](#BKMK_ManuallyConfigureDNS), più avanti in questo argomento.  
  
4.  Verificare che il dominio è in uso in Office 365.  
  
     È necessario attendere dopo aver completato la procedura guidata, mentre le registrar di nomi di dominio verifica i record DNS. Ciò avviene automaticamente; non devi eseguire alcuna operazione. Ma in genere richiede circa un'ora? e in alcuni casi un po' più tempo. Quando la verifica del dominio è completa, il **Office 365** pagina elenca il dominio dell'organizzazione.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a> Come configurare manualmente i record DNS per l'integrazione di Office 365  
 Se la procedura guidata Collega il dominio a Office 365 rileva che il nome di dominio non è gestito dal server, per completare la configurazione, è necessario configurare manualmente i record DNS (Domain Name Server) necessari. In tal caso, si troverà un elenco di record DNS che è necessario configurare **%username%\NewDNSRecords_ (n). txt**, dove *(n)* è un numero casuale.  
  
 La tabella seguente descrive i record DNS che è necessario aggiungere. I metodi di immissione possono variare a seconda del registrar di nomi di dominio. In caso di domande, rivolgersi al registrar di nomi di dominio per assistenza.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Record DNS necessari per collegare un nome di dominio Internet personalizzato a Office 365  
  
|Servizio|Record DNS necessari|Scopo|  
|-------------|--------------------------|-------------|  
|(Più servizi)|MX| Office 365 Usa questo record per verificare che si è proprietari di un nome di dominio specifico. Questo record MX non interferisce con il routing dei messaggi di posta elettronica.|  
|Exchange Online|MX|Fornisce il routing dei messaggi di posta elettronica. **Importante:**  Se si deve eseguire la migrazione della posta elettronica, non assegnare una preferenza zero (**0**) al nuovo record MX. Assicurarsi che il valore del record sia maggiore del valore assegnato al record MX corrente. Quando è stata completata la migrazione della posta elettronica e si è pronti per cambiare il server di posta elettronica a Office 365, hanno le registrar di nomi di dominio di reimpostare il valore di preferenza del nuovo record MX.|  
|Exchange Online|Alias (CNAME)|Record di individuazione automatica usato per aiutare gli utenti a configurare facilmente una connessione tra Exchange Online e un client di posta elettronica mobile o il client desktop Outlook. **Nota:**  Se si preferisce accedere a Outlook Web Access con il nome di dominio dell'organizzazione (ad esempio, http://mail.contoso.com) invece dell'URL standard (https://outlook.com/owa/office365.com), è possibile configurare il record Alias (CName) come indicato di seguito: **Type=CNAME, TTL=01:00:00, HostName=mail, Address=mail.office365.com**|  
|Exchange Online|TXT|Specifica che outlook.com, il dominio utilizzato dal server di posta elettronica di Office 365, è autorizzato a inviare posta elettronica per conto del dominio. Creare questo record per evitare che la posta elettronica in uscita venga contrassegnata come posta indesiderata.|  
|Lync Online|SRV|Aiuta ad abilitare la federazione con altri servizi di messaggistica istantanea come Windows Live o Yahoo!.|  
|Lync Online|SRV|Record di individuazione automatica usato per aiutare gli utenti a configurare facilmente una connessione tra il client desktop Lync e Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Dopo il dominio verifica è completa, non tentare di aggiungere o apportare altre modifiche ai record DNS dal portale di Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a> Passaggio successivo  
  
-   Creare account di Microsoft Online Services per gli utenti  
  
     Per usare servizi di Office 365, gli utenti devono avere un account Microsoft Online Services associato con l'account utente di rete. Con il dashboard è semplice. Se si usa una nuova sottoscrizione di Office 365, è possibile creare in blocco gli account online per gli account utente esistenti. Se si integra un nuovo server con una sottoscrizione di Office 365 che è già in uso, è possibile importare gli account utente dagli account online esistenti. Per le procedure, vedere [gestire account Online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  Nel Dashboard in Windows Server Essentials, account Microsoft Online Services sono indicati come account di Office 365. È cambiata solo la terminologia, mentre gli account sono rimasti invariati.  
  
##  <a name="BKMK_ManageIntegration"></a> Gestire l'integrazione di Office 365  
 Dopo aver integrato il server con Office 365, il **Office 365** pagina del dashboard Visualizza informazioni sulla sottoscrizione di Office 365 e rende disponibili queste attività:  
  
-   [Gestire la sottoscrizione di Office 365](#BKMK_ManageO365) ? Modificare l'account amministratore che consente di gestire la sottoscrizione. Aprire il Dashboard di amministrazione di Office 365 per gestire la sottoscrizione.  
  
-   [Collegamento di dominio Internet dell'organizzazione a Office 365](#BKMK_StepThree) ? Se si desidera essere in grado di inviare e ricevere posta elettronica indirizzata al proprio dominio, è possibile collegare il dominio a Office 365. (Illustrato in precedenza in [passaggio 3: Collegare il dominio dell'organizzazione a Office 365](#BKMK_StepThree).)  
  
-   [Disabilitare l'integrazione di Office 365](#BKMK_Disable) ? Se non si desidera gestire i servizi di Office 365, sottoscrizione e gli account online dal Dashboard, è possibile disabilitare l'integrazione di Office 365. I servizi rimangono disponibili nel portale di Office 365.  
  
###  <a name="BKMK_ManageO365"></a> Gestire la sottoscrizione di Office 365  
 Se è necessario apportare modifiche all'abbonamento a Office 365, mentre si lavora sul server, è possibile aprire la sottoscrizione di Office 365 dal **Office 365** page del Dashboard. È anche possibile modificare l'account amministratore che il server utilizza per apportare modifiche ai servizi Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Per aprire l'abbonamento nel dashboard di amministrazione di Office 365  
  
1.  Nel Dashboard di Windows Server Essentials, aprire il **Office 365** pagina.  
  
2.  In **Attività di configurazione**fare clic su **Gestione di Office 365**.  
  
3.  Accedere a Office 365 con l'account online Microsoft che consente di gestire la sottoscrizione.  
  
     Verrà aperto il Dashboard di amministrazione di Office 365.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Per cambiare l'account amministratore di Office 365  
  
1.  Nel dashboard fare clic su **Office 365**.  
  
2.  In **Attività di configurazione**fare clic su **Modifica l'account dell'amministratore di Office 365**. Viene visualizzata la procedura guidata Cambia account amministratore. (In Windows Server Essentials, la procedura guidata è denominata impostare Configura Account di Office 365 Administrator.)  
  
3.  Digitare le credenziali per l'account che si desidera usare per connettersi alla sottoscrizione di Office 365 e quindi fare clic su **successivo**.  
  
4.  Fare clic su **Chiudi**. Il dashboard viene riavviato.  
  
###  <a name="BKMK_Disable"></a> Disabilitare l'integrazione di Office 365  
 Se si decide che non si desidera gestire i servizi di Office 365 e gli account online dal Dashboard, è possibile disabilitare l'integrazione di Office 365. Sottoscrizione di Office 365 rimane attiva e tutte le modifiche di configurazione apportate dal Dashboard restano invariate. Ad esempio, si riceverà la posta elettronica indirizzata a un nome di dominio che è collegato alla sottoscrizione di Office 365. Non perdere alcun messaggio di posta elettronica e i controlli impostati per i dispositivi mobili sono ancora usati per Exchange Online.  
  
 In futuro, si gestiranno la sottoscrizione di Office 365, servizi e le risorse di Office 365 e gli utenti dovranno gestire le password per gli account online in Office 365. La sincronizzazione delle password non viene più eseguita e la disabilitazione o la rimozione di un account utente non avrà alcun effetto sull'account online dell'utente.  
  
 Poiché il software di integrazione di Office 365 è installato nel server locale, è possibile disabilitare la funzionalità anche se il servizio di integrazione non può connettersi a Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Per disabilitare l'integrazione di Office 365 con il server  
  
1.  Nel dashboard fare clic su **Office 365**.  
  
2.  Fare clic su **Disabilita l'integrazione di Office 365**. Viene visualizzata la disabilitazione guidata di Office 365.  
  
3.  Seguire le istruzioni per completare la procedura guidata.  
  
> [!NOTE]
>  Per abilitare nuovamente l'integrazione di Office 365, usare il **integra con Office 365** attività nel **Service** scheda del dashboard **Home** pagina. Per istruzioni, vedere [passaggio 2: Integrare il server di Windows Server Essentials con Microsoft Office 365](#BKMK_StepTwo), più indietro in questo argomento.  
  
##  <a name="BKMK_Troubleshoot"></a> Risolvere i problemi di integrazione di Office 365  
 In questa sezione fornisce informazioni che consentono di risolvere i problemi comuni che possono verificarsi quando si usano le funzionalità di integrazione di Office 365 in Windows Server Essentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a> Non sono stati creati alcuni account di Microsoft Online Services  
 **Descrizione**  
  
 Un tentativo di creare uno o più account Microsoft Online Services dal Dashboard non è riuscito.  
  
 **Soluzione**  
  
1.  Fare clic sul collegamento nella pagina di completamento della procedura guidata per aprire un file di risultati contenente informazioni dettagliate su ogni richiesta di creazione account non completata. Ad esempio, un risultato potrebbe informare che esiste già un account di Microsoft Online Services con il nome di un account richiesto.  
  
2.  Eseguire le azioni consigliate per risolvere ogni errore.  
  
3.  Se il problema persiste, riavviare il server e quindi provare nuovamente a create gli account online.  
  
###  <a name="BKMK_ProblemUninstalling"></a> Si è verificato un problema durante la disinstallazione di integrazione di Office 365  
 **Descrizione**  
  
 Quando si è provato a disabilitare l'integrazione di Office 365, si è verificato un errore sconosciuto.  
  
 **Soluzione**  
  
1.  Verificare che il computer sia connesso a Internet e quindi riprovare.  
  
2.  Se l'errore si verifica ancora, riavviare il server e quindi riprovare.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica dell'integrazione dei servizi per Windows Server Essentials - parte 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Panoramica dell'integrazione dei servizi per Windows Server Essentials - parte 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guida introduttiva all'uso di Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gestire Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
