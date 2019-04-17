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
ms.openlocfilehash: b45bddb657b96c7cc5f9291a6c887b9d0801974b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gestire Office 365 in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si integra il server Windows Server Essentials con Microsoft Office 365, è possibile gestire i servizi di Office 365 e gli account online insieme risorse locali dal Dashboard di Windows Server Essentials. In questo argomento, tutto è scoprire i vantaggi dell'integrazione del server con Office 365, eseguire la procedura e come gestire e risolvere i problemi dell'integrazione di Office 365.  
  
  
  
> [!IMPORTANT]
>   Integrazione di Office 365 è supportata solo in un ambiente di controller di dominio singolo. È inoltre necessario eseguire la procedura guidata integrazione con Office 365 in un controller di dominio.  
  
## <a name="in-this-topic"></a>In questo argomento  
  
-   [Perché integrare Office 365 con il mio server?](#BKMK_IntegrationOverview)  
  
-   [Configurare l'integrazione di Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gestire l'integrazione con Office 365](#BKMK_ManageIntegration)  
  
-   [Risolvere i problemi di integrazione di Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a>Perché integrare Office 365 con il mio server?  
 Ci sono numerosi motivi validi per integrare Office 365 con il server Windows Server Essentials. Se si gestiscono alcune risorse internamente, ma usare Office 365 per altri servizi, verrà visualizzata in grado di gestire i servizi di Office 365 e le risorse dal Dashboard, insieme le risorse locali, invece di lavorare in due posizioni diverse.  
  
-   Gestire gli account online che concedono agli utenti l'accesso a Office 365 insieme agli account utente:  
  
    -   Creare account Microsoft Online Services per gli utenti in un unico passaggio oppure creare gli account utente sul server per gli account online esistenti. È anche possibile aggiungere un account online a un account utente nuovo o esistente.  
  
         Quando si creano gli account online dal Dashboard, gli utenti l'accesso a Office 365 con la stessa password usata sul server. Se si modifica la password per l'account utente, viene modificato anche la password online. E si è certi che le password dell'account online è sempre soddisfino i requisiti di sicurezza impostati per gli account utente.  
  
    -   Gestire un account online insieme all'account utente in tutto il ciclo di vita dell'account utente. Se si disattiva un account utente, viene disattivato anche l'account online. Se si rimuove un account utente, viene rimosso anche l'account online.  
  
    -   In un server Windows Server Essentials, gestire anche i gruppi di distribuzione Exchange Online per la posta elettronica.  
  
-   Inviare e ricevere posta elettronica dal dominio Internet dell'organizzazione s (ad esempio, contoso.com) collegando un dominio Internet personalizzato all'abbonamento a Office 365.  
  
-   Gestire l'abbonamento e l'integrazione di Office 365 dal Dashboard.  
  
-   Se la sottoscrizione include raccolte di SharePoint Online, integrazione con Office 365 con un server Windows Server Essentials consente di:  
  
    -   Creare e gestire raccolte di SharePoint Online dal Dashboard.  
  
        > [!NOTE]
        >  Ll è anche possibile usare l'app My Server 2012 R2 per lavorare con i documenti nelle raccolte di SharePoint Online dal portatile, un dispositivo mobile o Windows phone. Questa funzionalità è disponibile solo per Windows Server Essentials. Per ulteriori informazioni, vedere [usare l'App My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
    -   Modificare le autorizzazioni per un sito del team di SharePoint Online dal Dashboard o aprire il sito del team dal Dashboard per apportare altre modifiche.  
  
     Per ulteriori informazioni, vedere [gestire SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
-   Se si sottoscrive Exchange Online, integrazione con Office 365 con un server Windows Server Essentials consente di gestire i dispositivi mobili che gli utenti usano per connettersi al server di posta elettronica aziendale:  
  
    -   Richiedere la protezione tramite password quando i dispositivi mobili si connettono al server di posta elettronica della società. Impostare una lunghezza minima della password, il numero di tentativi di accesso non riusciti per consentire e il tempo minimo richiesto tra tentativi di accesso.  
  
    -   Bloccare un dispositivo mobile di connettersi a Exchange Online se si conosce del problemi di sicurezza con il modello del dispositivo.  
  
    -   Se un dispositivo mobile viene perso o rubato, cancellare il dispositivo per eliminare i dati sensibili della società la prossima volta che il dispositivo è attivato.  
  
-    Integrazione di Office 365 offre nuovi modi per connettersi alle risorse e servizi di Office 365:  
  
    -   Apri la finestra di avvio di Windows Server Essentials servizi di Office 365. Per informazioni, vedere [Guida introduttiva all'uso di Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
    -   Usare l'app My Server 2012 R2 per lavorare con i documenti nelle raccolte di SharePoint Online dal portatile, un dispositivo mobile o Windows phone. Per informazioni, vedere [usare l'App My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Questa funzionalità è disponibile solo in Windows Server Essentials.  
  
##  <a name="BKMK_Configure"></a>Configurare l'integrazione di Office 365  
 È possibile integrare il server con Office 365 in qualsiasi momento dopo aver completato l'installazione del server. Se non si t dispone già di un abbonamento a Office 365, è possibile acquistarne una o registrarsi per una sottoscrizione di valutazione gratuita.  
  
 Si eseguiranno le attività seguenti:  
  
-   [Passaggio 1: Verificare i requisiti di integrazione di Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Passaggio 2: Integrare il server con Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Passaggio 3: Collegare il nome di dominio Internet dell'organizzazione s a Office 365 (facoltativo)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a>Passaggio 1: Verificare i requisiti di integrazione di Office 365  
 Prima di iniziare, verificare che il server soddisfi questi requisiti:  
  
-   Il server può avere uno di questi sistemi operativi: Windows Server Essentials, Windows Server Essentials o il sistema operativo Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato.  
  
-   L'ambiente può avere solo un controller di dominio, ed è necessario eseguire l'integrazione di Office 365 nel controller di dominio.  
  
-   È necessario essere in grado di connettersi a Internet dal server.  
  
-   Prima di avviare l'integrazione con Office 365, è necessario installare tutti gli aggiornamenti critici e importanti nel server.  
  
-   Se si desidera utilizzare il dominio Internet s organizzazione negli indirizzi di posta elettronica e con risorse di SharePoint Online, è necessario registrare il nome di dominio in modo che è possibile collegare il dominio a Office 365 durante l'integrazione. Per ulteriori informazioni, vedere [come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Non si non è necessario sottoscrivere in anticipo Office 365. Ll è in grado di acquistare un abbonamento o iscriversi per una valutazione gratuita durante l'integrazione di Office 365. Se vuoi d ottenere un quadro piani e ai prezzi per Office 365, [confrontare i piani di Office 365 per le aziende](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a>Passaggio 2: Integrare il server con Microsoft Office 365  
 Eseguire la procedura seguente nel controller di dominio per integrare il server Windows Server Essentials con Office 365.  
  
> [!NOTE]
>  La procedura s lo stesso che tu abbia Windows Server Essentials o Windows Server Essentials, ma è tutto avviare da una posizione diversa della Home Page. In Windows Server Essentials, si integra il server con Office 365 e altri servizi Microsoft Online nel **servizi** scheda. In Windows Server Essentials, viene eseguita l'integrazione di Office 365 sul **posta elettronica** scheda.  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Per integrare il server con Office 365  
  
1.  Accedere al server come amministratore e aprire il Dashboard di Windows Server Essentials.  
  
2.  Nel **Home** pagina, fare clic su **servizi** (in Windows Server Essentials, fare clic su **posta elettronica**), fare clic su **integra con Microsoft Office 365**, quindi fare clic su **configurare l'integrazione di Microsoft Office 365**.  
  
     Viene visualizzata l'integrazione guidata con Microsoft Office 365.  
  
3.  Nel **iniziare** pagina, effettuare una delle operazioni seguenti:  
  
    -   Se si don t ha un abbonamento a Office 365, fare clic su **Avanti**, seguire le istruzioni per la sottoscrizione a Office 365 o accesso backup per una sottoscrizione di valutazione.  
  
         È necessario accedere a Office 365 prima di tornare alla procedura guidata. Ma non si non è necessario eseguire le attività nel **inizia da qui** sezione del portale di Office 365.  
  
    -   Se hai già un abbonamento a Office 365 che si desidera integrare con il server, selezionare **ha già una sottoscrizione a Office 365**, quindi fare clic su **Avanti**.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
 Dopo la procedura guidata viene completata correttamente, verrà visualizzata notare le modifiche seguenti al Dashboard:  
  
-   Qui s un nuovo **Office 365** pagina, che viene utilizzato per gestire l'integrazione e l'abbonamento a Office 365.  
  
-   Nel **utenti** pagina, puoi vedere tutto un **gli account Online** scheda in cui è possibile creare e gestire gli account di Microsoft Online Services che danno agli utenti l'accesso a Office 365. Se si stanno utilizzando Exchange Online e dispone di un server Windows Server Essentials, è anche visualizzato un **gruppi di distribuzione** scheda.  
  
-   Il **archiviazione** pagina in un server Windows Server Essentials include una **raccolte di SharePoint** scheda per la gestione di raccolte di SharePoint Online e la modifica delle autorizzazioni per i siti del team. Ogni piano aziendale per Office 365 include queste funzionalità di SharePoint Online.  
  
###  <a name="BKMK_StepThree"></a>Passaggio 3: Collegare il nome di dominio Internet dell'organizzazione s a Office 365 (facoltativo)  
 Se si desidera utilizzare il proprio dominio Internet nella posta elettronica indirizzata all'organizzazione e negli URL delle risorse di SharePoint Online, è possibile collegare un dominio personalizzato all'abbonamento a Office 365. Se si integra il server Windows Server Essentials con Office 365, è possibile eseguire dal Dashboard.  
  
 È più semplice eseguire questa operazione prima di creare online s account per gli utenti in modo che è possibile utilizzare il dominio quando creare in blocco gli account online.  
  
 Ottenere un nome di dominio quando si esegue l'iscrizione per Office 365?, ad esempio, *contoso*. onmicrosoft.com. Se d invece usi un altro nome di dominio? pronuncia semplicemente contoso.com? è possibile. È necessario acquistare un nome di dominio se non si t proprio già uno e cambiare alcuni record DNS.  
  
 Configurazione di un dominio personalizzato da usare con Office 365 include quattro attività:  
  
1.  **Acquistare un nome di dominio.** Ovvero registrarlo con un registrar o provider di hosting DNS.  
  
    -   Scegliere un nome di dominio che funziona con Office 365. È possibile utilizzare un nome di dominio di livello 2 °?, ad esempio, buycontoso.com?, ma non un nome di dominio di livello 3?, ad esempio, marketing.contoso.com. Per ulteriori informazioni sulla scelta di un dominio da usare in Office 365, vedere [domini](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
    -   Acquistarlo da un registrar che consente i record domain name server (DNS) necessari per Office 365. Per conoscere i Registrar che consentono i record DNS richiesti, vedere [come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Se già registrato il dominio con un altro registrar, non si comunque; è possibile trasferire il dominio a altro registrar quando si collega il dominio a Office 365.  
  
2.  **Configurare record DNS che consentano di utilizzare il nome di dominio servizi di Office 365.** Il modo più semplice è la procedura guidata configurare automaticamente i record DNS per te quando si collega il dominio all'abbonamento a Office 365 nel passaggio 3. Se si d invece eseguire tale operazione manualmente, vedere [come configurare manualmente DNS dei record per l'integrazione con Office 365](#BKMK_ManuallyConfigureDNS).  
  
3.  **Collegare il dominio Internet personalizzato all'abbonamento a Office 365.** È possibile utilizzare tutto il **collega un dominio a Office 365** azione.  
  
4.  **Verificare che i servizi di Office 365 usino il nuovo nome di dominio.**  
  
 Se si completano i passaggi 1 e 2 prima di usare il **collega un dominio a Office 365** attività, la procedura guidata può collegare il nome di dominio a Office 365. In alternativa, è possibile ricorrere a una procedura guidata con alcuni o tutti i passaggi 1 e 2.  
  
##### <a name="to-link-your-organization-s-internet-domain-to-office-365"></a>Per collegare il dominio Internet dell'organizzazione s a Office 365  
  
1.  Nel Dashboard, aprire il **Office 365** pagina e fare clic su **collega un dominio a Office 365**.  
  
2.  Seguire le istruzioni per completare la procedura guidata.  
  
     La procedura guidata può aiutare con alcuni o tutti i passaggi della registrazione, configurazione e collegamento di un nome di dominio Internet nuovo o esistente da utilizzare in Office 365.  
  
     Fare clic sul collegamento Guida nella pagina della procedura guidata per ottenere le informazioni necessarie per completare un'attività. Oppure vedere la configurazione di una sezione del nome di dominio di [gestire accesso Web remoto in Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) per una panoramica del processo e i requisiti.  
  
    > [!NOTE]
    >  Per utilizzare la procedura guidata per registrare un nuovo nome di dominio, è necessario utilizzare uno dei provider di servizi di nomi di dominio che collaborano con Microsoft per offrire una facile integrazione con la procedura guidata. Per trovare un registrar di nomi di dominio, vedere [come acquistare un nome di dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Se la procedura guidata rileva che la t nome di dominio gestiti dal server, è necessario configurare manualmente i record DNS necessari per completare la configurazione verrà visualizzata. Per istruzioni, vedere [come configurare manualmente DNS dei record per l'integrazione con Office 365](#BKMK_ManuallyConfigureDNS), più avanti in questo argomento.  
  
4.  Verificare che il dominio è in uso in Office 365.  
  
     Qui s necessario attendere dopo aver completato la procedura guidata, mentre il registrar di nomi di dominio verifica i record DNS. Questo avviene automaticamente. Puoi tenere sempre è necessario eseguire alcuna operazione. Ma in genere richiede circa un'ora? e poco più. Quando si verifica del dominio è completa, il **Office 365** pagina verrà visualizzato il dominio dell'organizzazione s.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a>Come configurare manualmente i record DNS per l'integrazione con Office 365  
 Se la collega il dominio guidata di Office 365 rileva che il nome di dominio non è gestito dal server, per completare la configurazione, è necessario configurare manualmente i record necessari domain name server (DNS). In tal caso, verrà visualizzata trovare un elenco di record DNS che è necessario configurare in **%username%\NewDNSRecords_ (n). txt**, dove *(n)* è un numero casuale.  
  
 La tabella seguente descrive i record DNS che è necessario aggiungere. I metodi di immissione possono variare a Registrar di nomi di dominio diverso. Se hai domande, chiedi aiuto registrar di nomi di dominio.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Record DNS necessari per il collegamento di un nome di dominio Internet personalizzato a Office 365  
  
|Servizio|Record DNS necessari|Scopo|  
|-------------|--------------------------|-------------|  
|(Più servizi)|MX| Office 365 Usa questo record per verificare che si è proprietari di un nome di dominio specifico. Questo record MX non interferisce con routing dei messaggi di posta elettronica.|  
|Exchange Online|MX|Fornisce il routing dei messaggi e-mail. **Importante:** se la migrazione di posta elettronica, non assegnare una preferenza zero (**0**) per il nuovo record MX. Assicurarsi che il valore del record sia maggiore del valore assegnato al record MX corrente. Quando la migrazione della posta elettronica è completa e si è pronti a sostituire il server di posta elettronica a Office 365, è necessario il registrar reimpostare il valore della preferenza del nuovo record MX.|  
|Exchange Online|Alias (CNAME)|Record di individuazione automatica usato per aiutare gli utenti facilmente impostare una connessione tra Exchange Online e il client desktop Outlook o un client di posta elettronica per dispositivi mobili. **Nota:** se si preferisce accedere a Outlook Web Access con il nome di dominio dell'organizzazione s (ad esempio, http://mail.contoso.com) invece dell'URL standard (https://outlook.com/owa/office365.com), è possibile configurare il record Alias (CName) come indicato di seguito: **tipo = CNAME, TTL = 01: 00:00, nome host = posta, indirizzo = mail.office365.com**|  
|Exchange Online|TXT|Specifica che outlook.com, il dominio usato dai server di posta elettronica di Office 365, è autorizzato a inviare posta elettronica per conto del dominio. Creare questo record per evitare la posta elettronica in uscita venga contrassegnata come posta indesiderata.|  
|Lync Online|SRV|Consente di abilitare la federazione con altri servizi di messaggistica istantanea come Windows Live o Yahoo!.|  
|Lync Online|SRV|Record di individuazione automatica usato per aiutare gli utenti facilmente impostare una connessione tra il client desktop Lync e Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Dopo aver dominio verifica è completa, non tentare di aggiungere o apportare altre modifiche ai record DNS dal portale di Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a>Passaggio successivo  
  
-   Creare account Microsoft Online Services per gli utenti.  
  
     Per utilizzare servizi di Office 365, gli utenti devono avere un account Microsoft Online Services associato all'account utente di rete. Il Dashboard è semplice. Se si stanno utilizzando un nuovo abbonamento a Office 365, è possibile creare in blocco gli account online per gli account utente esistente. Se si stanno integra un nuovo server con un abbonamento a Office 365 che si stanno già in uso, è possibile importare gli account utente dal tuo online account. Per le procedure, vedere [gestire account Online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  Nel Dashboard di Windows Server Essentials, gli account di Microsoft Online Services sono indicati come account di Office 365. Gli account sono uguali. solo la terminologia modificata.  
  
##  <a name="BKMK_ManageIntegration"></a>Gestire l'integrazione con Office 365  
 Dopo aver integrato il server con Office 365, la **Office 365** pagina del dashboard vengono visualizzate le informazioni sull'abbonamento a Office 365 e rende disponibili queste attività:  
  
-   [Gestire l'abbonamento a Office 365](#BKMK_ManageO365) ? Cambiare l'account amministratore utilizzato per gestire la sottoscrizione. Aprire il Dashboard di amministrazione di Office 365 per gestire la sottoscrizione.  
  
-   [Collegare il dominio Internet dell'organizzazione s a Office 365](#BKMK_StepThree) ? Se si desidera essere in grado di inviare e ricevere posta elettronica indirizzata al proprio dominio, è possibile collegare il dominio a Office 365. (Descritto in precedenza in [passaggio 3: collegare il dominio dell'organizzazione s a Office 365](#BKMK_StepThree).)  
  
-   [Disabilitare l'integrazione di Office 365](#BKMK_Disable) ? Se si don t desidera gestire i servizi di Office 365, una sottoscrizione e un account online dal Dashboard, è possibile disabilitare l'integrazione di Office 365. I servizi rimangono disponibili nel portale di Office 365.  
  
###  <a name="BKMK_ManageO365"></a>Gestire l'abbonamento a Office 365  
 Se è necessario apportare modifiche all'abbonamento a Office 365 mentre si re lavoro sul server, è possibile aprire l'abbonamento in Office 365 dal **Office 365** pagina del Dashboard. È inoltre possibile modificare l'account amministratore utilizzato dal server per apportare modifiche ai servizi di Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Per aprire l'abbonamento nel Dashboard di amministrazione di Office 365  
  
1.  Nel Dashboard di Windows Server Essentials, aprire il **Office 365** pagina.  
  
2.  In **attività di configurazione**, fare clic su **gestire Office 365**.  
  
3.  Accedere a Office 365 con l'account online Microsoft che usi per gestire la sottoscrizione.  
  
     Apre il Dashboard di amministrazione di Office 365.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Per modificare l'account di amministratore di Office 365  
  
1.  Nel Dashboard, fare clic su **Office 365**.  
  
2.  In **attività di configurazione**, fare clic su **cambiare l'account di amministratore di Office 365**. Verrà visualizzata la procedura guidata Account amministratore cambia. (In Windows Server Essentials, la procedura guidata è denominata impostare Configura Account di Office 365 amministratore.)  
  
3.  Digitare le credenziali per l'account che si desidera utilizzare per connettersi all'abbonamento a Office 365 e quindi fare clic su **Avanti**.  
  
4.  Fare clic su **Chiudi**. Il Dashboard viene riavviato.  
  
###  <a name="BKMK_Disable"></a>Disabilitare l'integrazione di Office 365  
 Se si decide che t non si desidera gestire i servizi di Office 365 e gli account online dal Dashboard, è possibile disabilitare l'integrazione di Office 365. L'abbonamento a Office 365 rimane attiva e tutte le modifiche di configurazione apportate dal Dashboard restano invariate. Ad esempio, si ll ricevere posta elettronica indirizzata a un nome di dominio collegato all'abbonamento a Office 365. Si vincente t perdere qualsiasi messaggio di posta elettronica e i controlli impostati per i dispositivi mobili sono ancora usati per Exchange Online.  
  
 Da ora in poi, si gestiranno l'abbonamento a Office 365, servizi e le risorse in Office 365 e gli utenti dovranno gestire le password per gli account online in Office 365. Sincronizzazione password non viene più eseguita e la disabilitazione o la rimozione di un account utente non avrà alcun effetto sull'account utente s online.  
  
 Poiché il software di integrazione di Office 365 è installato nel server locale, è possibile disabilitare la funzionalità anche se il servizio di integrazione non può connettersi a Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Per disabilitare l'integrazione di Office 365 con il server  
  
1.  Nel Dashboard, fare clic su **Office 365**.  
  
2.  Fare clic su **disabilitare l'integrazione di Office 365**. Verrà visualizzata la disabilitazione guidata 365 Office.  
  
3.  Seguire le istruzioni per completare la procedura guidata.  
  
> [!NOTE]
>  Per abilitare nuovamente l'integrazione di Office 365, usare il **integra con Office 365** attività sul **servizio** scheda del Dashboard s **Home** pagina. Per istruzioni, vedere [passaggio 2: integrare il server Windows Server Essentials con Microsoft Office 365](#BKMK_StepTwo), più indietro in questo argomento.  
  
##  <a name="BKMK_Troubleshoot"></a>Risolvere i problemi di integrazione di Office 365  
 Questa sezione fornisce informazioni che consentono di risolvere i problemi comuni che possono verificarsi quando si utilizza la funzionalità di integrazione di Office 365 in Windows Server Essentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a>Non sono stati creati alcuni account Microsoft Online Services  
 **Descrizione**  
  
 Un tentativo di creare uno o più account di Microsoft Online Services dal Dashboard stato t esito positivo.  
  
 **Soluzione**  
  
1.  Fare clic sul collegamento nella pagina di completamento della procedura guidata per aprire un file di risultati contenente informazioni dettagliate su ogni richiesta di creazione account non è stato completato. Ad esempio, un risultato potrebbe informare che esiste già un account Microsoft Online Services con il nome di un account richiesto.  
  
2.  Eseguire le azioni consigliate per risolvere ogni errore.  
  
3.  Se il problema persiste, riavviare il server e quindi provare a creare di nuovo gli account online.  
  
###  <a name="BKMK_ProblemUninstalling"></a>Si è verificato un problema di disinstallazione dell'integrazione di Office 365  
 **Descrizione**  
  
 Si è verificato un errore sconosciuto durante il tentativo di disabilitare l'integrazione di Office 365.  
  
 **Soluzione**  
  
1.  Assicurarsi che il computer è connesso a Internet e quindi riprovare.  
  
2.  Se l'errore si verifica ancora, riavviare il server e quindi riprovare.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica dell'integrazione dei servizi per Windows Server Essentials - parte 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Panoramica dell'integrazione dei servizi per Windows Server Essentials - parte 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guida introduttiva all'uso di Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gestire Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
