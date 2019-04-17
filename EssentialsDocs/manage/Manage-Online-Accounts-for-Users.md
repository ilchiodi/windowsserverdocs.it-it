---
title: Gestire gli account Online per gli utenti di Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
8author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 95f401bbec9bb503d19e2d9918a05851c04f7ef4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Gestire gli account Online per gli utenti di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si integra il server Windows Server Essentials con Microsoft Office 365, è possibile gestire gli account online insieme agli account utente dal Dashboard. In questo argomento, verrà visualizzata scoprire cosa è possibile ottenere Gestione account di Microsoft Online Services gli utenti dal Dashboard, come creare e gestire gli account online dal Dashboard e come gestire indirizzi di posta elettronica e i gruppi di distribuzione per Exchange Online dal Dashboard.  

  
> [!NOTE]
>  Per gestire gli account di Microsoft Online Services in Windows Server Essentials, è necessario integrare il server con Office 365. Per istruzioni, vedere [gestire Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Se si gestiscono gli account online in Windows Server Essentials, è abituati a vedere gli account di Microsoft Online Services detto *account Office 365*. Nel Dashboard di Windows Server Essentials, le etichette sono state modificate in *account Microsoft Online Services*, o *account online Microsoft* forma abbreviata. Gli account e le procedure sono uguali. solo le etichette modificate. La maggior parte delle procedure descritte in questo argomento viene utilizzato il termine *account online*.  
  
## <a name="in-this-topic"></a>In questo argomento  
  
-   [Vantaggi della gestione degli account online dal Dashboard?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Creare account online](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Gestire gli account online](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Gestire indirizzi di posta elettronica per Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Gestire gruppi di distribuzione per Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a>Vantaggi della gestione degli account online dal Dashboard?  
 Quando si usa il Dashboard per assegnare un account Microsoft Online Services a un account utente, le password degli account vengono sincronizzate automaticamente, ed è possibile mantenere i due account tutto il ciclo di vita s account utente.  
  
 È utile per l'utente, che è possibile utilizzare la stessa password per accedere alle risorse sul server e in Office 365. Ed è possibile applicare gli stessi requisiti delle password per l'accesso alle risorse di Office 365 necessari per le risorse interne.  
  
### <a name="how-does-password-synchronization-work"></a>Come funziona la sincronizzazione delle password?  
 Quando si usa il Dashboard per assegnare un account Microsoft Online Services a un account utente, la password dell'account utente viene sincronizzata automaticamente con l'account online s. Ciò significa che un utente deve solo una sola password per accedere alle risorse sul server e in Office 365. Inoltre, è possibile utilizzare lo stesso nome per l'account utente e ID utente s online.  
  
 Password vengono sincronizzate immediatamente e automaticamente quando un utente cambia la password per l'account utente da un computer aggiunto al dominio o mediante accesso Web remoto.  
  
> [!IMPORTANT]
>  Se l'integrazione di Office 365 con Windows Server Essentials, gli utenti non devono cambiare la password per l'account online Microsoft dal portale di Office 365. In questo modo verrà interrotta la sincronizzazione delle password.  
  
### <a name="simplified-account-creation"></a>Creazione di account semplificata  
 Ecco un altro vantaggio quando si creano gli account online iniziali dal Dashboard. È possibile creare account online per tutti gli utenti con un'unica azione. D'altra parte, se i dipendenti usano già Office 365 e configurazione di un nuovo server Windows Server Essentials, è possibile creare tutti gli account utente dagli account online con un'unica azione. Per ulteriori informazioni, vedere [creare account online](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Gestire indirizzi di posta elettronica e gruppi di distribuzione dal Dashboard  
 Sarà in grado di gestire gli indirizzi di posta elettronica e i gruppi di distribuzione per Exchange Online dal Dashboard. Ed è possibile utilizzare il dominio Internet s organizzazione negli indirizzi di posta elettronica. È possibile eseguire tutte queste dal Dashboard, senza accedere a Office 365. (È necessario utilizzare Windows Server Essentials per gestire i gruppi di distribuzione dal Dashboard. Questa funzionalità non è supportata in Windows Server Essentials.) Per ulteriori informazioni, vedere [gestire indirizzi di posta elettronica per Exchange Online](#BKMK_SECTION_ManageEmailAddresses) e [gestire gruppi di distribuzione per Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Gestire l'account utente e account online insieme  
 Ed è possibile gestire un account online insieme all'account utente in tutto il ciclo di vita di account s. Se si disattiva l'account utente, l'account online viene disattivato anche in Microsoft Online Services. Se si rimuove un account utente, viene rimosso anche l'account online. Per ulteriori informazioni, vedere [gestire account online](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a>Creare account online  
 Dopo aver integrato il server con Office 365, è possibile creare account Microsoft Online Services per gli utenti dal Dashboard comodamente. Ll si hanno a disposizione più flessibilità nella creazione degli account online. Se hai un nuovo abbonamento a Office 365, è possibile creare in blocco gli account online per tutti gli utenti. Se è già stato creato gli account online in Office 365, non preoccuparti. Se è la configurazione di un nuovo server, è possibile creare gli account utente sul server importando gli account online. Ed è possibile assegnare un nuovo o un account online esistente quando si crea un singolo account utente o quando si aggiunge un account online a un account utente esistente.  
  
 **Requisiti relativi alle licenze** ti servirà una licenza utente per ogni account online creato. Controllare il **Office 365** pagina nel Dashboard per individuare il numero di licenze utente disponibile tramite l'abbonamento a Office 365. Se è necessario aggiungere ulteriori licenze utente, verrà visualizzata in grado di aprire l'abbonamento a Office 365 in Office 365 per eseguire questa operazione.  
  
 **Operazioni successive per gli utenti** dopo aver aggiunto un account online per un account utente, all'utente viene richiesto di modificare la password per l'account utente al successivo accesso. La nuova password viene sincronizzata immediatamente con l'account online. In seguito, è possibile utilizzare la password per accedere a Office 365 con proprio ID online.  
  
> [!IMPORTANT]
>  Comunicare agli utenti che non deve essere cambiata la password dell'account online in Office 365. La password verrà cambiata automaticamente ogni volta che modifica la password per l'account utente. Se si modifica la password online in Office 365, la sincronizzazione delle password verrà interrotta.  
  
 Utilizzare le procedure in questa sezione per:  
  
-   [Creare in blocco gli account online per gli account utente esistenti](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importare gli account utente dagli account Microsoft Online Services](#BKMK_ToImportUserAccounts)  
  
-   [Creare un nuovo account utente con un account online assegnato](#BKMK_ToCreateaNewUserAccount)  
  
-   [Assegnare un account online a un account utente](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Se si utilizza Windows Server Essentials, si noterà *account Office 365* anziché *account Microsoft Online Services* durante queste procedure. Il processo è lo stesso, ma la terminologia è cambiata in Windows Server Essentials.  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a>Per creare in blocco gli account online per gli account utente esistenti  
  
1.  Accedere al server come amministratore e aprire il Dashboard di Windows Server Essentials.  
  
2.  Nel Dashboard, aprire il **utenti** pagina.  
  
3.  In **attività utenti**, fare clic su **account online Microsoft aggiungere**.  
  
     Il **account aggiungere Microsoft Online Services** pagina della procedura guidata consente di visualizzare tutti gli account utente che non dispongono di un account online Microsoft. Per impostazione predefinita sono selezionati tutti gli account e viene suggerito il nome utente per l'ID online Microsoft. Se è stato collegato un dominio Internet personalizzato all'abbonamento a Office 365, tale dominio verrà usato per impostazione predefinita.  
  
4.  Nel **account aggiungere Microsoft Online Services** pagina, esaminare gli account che verranno creati. Ad esempio, si consiglia di cercare gli utenti che già dispongono di un account online con un ID online diverso e assicurarsi che sia selezionato il dominio che si desidera usare negli indirizzi di posta elettronica. Dopo avere apportato le modifiche necessarie, fare clic su **Avanti**.  
  
5.  Nel **licenze assegnare Microsoft Online Services** pagina, selezionare i servizi di Office 365 verrà utilizzato dagli utenti. Quando si fa clic **Avanti**, viene avviata la creazione di account.  
  
    > [!NOTE]
    >  Hai una licenza di servizio in Office 365 per ogni account online. È possibile controllare le licenze disponibili e, se necessario, aprire l'abbonamento per aggiungere licenze dal **Office 365** pagina del dashboard.  
  
6.  Informare gli utenti che ora dispongono di un account online Microsoft. Devono cambiare la password di account utente di rete prima dell'accesso a Office 365. Per istruzioni, vedere [per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a>Per iniziare a usare un nuovo account online Microsoft  
  
1.  Accedere al computer con account utente di rete.  
  
2.  Modificare la password dell'account utente. Per iniziare, premere Ctrl + Alt + Canc, quindi scegliere **modificare una password**.  
  
     Quando si modifica la password, la password viene sincronizzata con il nuovo account online. È ora possibile utilizzare la stessa password per accedere a Office 365.  
  
3.  [accedere a Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) utilizzando il nuovo ID online e la password dell'account utente.  
  
    > [!IMPORTANT]
    >  Non modificare la password dell'account online in Office 365. Che verrà interrotta la sincronizzazione delle password. La password online verrà aggiornata ogni volta che modifichi la password dell'account utente di rete.  
  
###  <a name="BKMK_ToImportUserAccounts"></a>Per importare gli account utente dagli account online esistenti  
  
1.  Nel Dashboard, aprire il **utenti** pagina.  
  
2.  In **attività utenti**, fare clic su **Importa account da Office 365**.  
  
     La pagina successiva mostra tutti gli account online per l'abbonamento a Office 365 che dispone di un account utente sul server. Per impostazione predefinita sono selezionati tutti gli account e viene suggerito l'ID online per il nome utente.  
  
3.  Per creare gli account utente:  
  
    1.  Apportare le modifiche necessarie per gli account utente proposti.  
  
    2.  Facoltativamente, fare clic sul collegamento per visualizzare le password temporanee che verranno assegnate agli account utente. È necessario fornire agli utenti la password temporanea insieme al nuovo nome account.  
  
         (Dopo aver creato gli account, si disponibili queste password sono riportate in questo file: *SystemDrive*\Users\\*Office365admin*\\*Nuovoutenteserver*. txt dove *Office365admin* è l'account usato per l'amministrazione di Office 365 nel server di rete e *Nuovoutenteserver* è il nome del nuovo account utente.)  
  
    3.  Fare clic su **Avanti** per creare gli account utente.  
  
4.  Comunicare agli utenti i nuovi account utente e le password temporanee che useranno per accedere il server per il primo œ tempo e che è necessario modificare la password dopo l'accesso. Per istruzioni per gli utenti, vedere [per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Assicurarsi che sappiano che la password per l'account online verrà sincronizzata con l'account utente per il futuro e gli utenti non devono cambiare la password online in Office 365.  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a>Per creare un nuovo account utente con un account online assegnato  
  
1.  Nel Dashboard, fare clic su **utenti**.  
  
2.  In **attività utenti**, fare clic su **aggiungere un account utente**. L'aggiunta guidata Account utente viene visualizzato.  
  
3.  Seguire le istruzioni per creare l'account utente.  
  
4.  Nel **assegnare un account Microsoft Online Services** pagina, creare un nuovo in linea di account per l'utente o assegnare un account online esistente:  
  
    -   Per creare un nuovo account online, fare clic su **creare un nuovo account Microsoft Online Services e assegnalo a questo account utente**e digitare un nome per l'account Microsoft Online Services (per impostazione predefinita, il nome utente viene utilizzato per l'ID online). Fare clic su **Avanti**.  
  
    -   Per assegnare un account online Microsoft esistente, fare clic su **assegnare un account Microsoft Online Services esistente a questo account utente**e selezionare un account esistente dall'elenco a discesa. Fare clic su **Avanti**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, gli account di Microsoft Online Services sono indicati come account di Office 365 nelle procedure guidate e nelle etichette del Dashboard.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
6.  Notifica all'utente che dovrà cambiare la password dell'account utente prima di può accedere a Office 365 con il nuovo account online. Per istruzioni, vedere [per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>Per assegnare un account online a un account utente  
  
1.  Nel Dashboard, fare clic su **utenti**.  
  
2.  Fare doppio clic su account utente nell'elenco, quindi fare clic su **assegnare un account online Microsoft**. Assegna un Account Microsoft Online Services Wizard verrà visualizzata.  
  
3.  Assegnare un account online esistente o crearne uno nuovo per l'utente. L'ID online predefinito per un nuovo account è il nome utente. Fare clic su **Avanti** per aggiungere l'account online per l'account utente.  
  
4.  Esaminare le informazioni nell'ultima pagina della procedura guidata, quindi fare clic su **Chiudi**.  
  
5.  Notifica all'utente che dovrà cambiare la password dell'account utente prima di può accedere a Office 365 con il nuovo account online. Per istruzioni, vedere [per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a>Gestire gli account online  
 Quando si aggiunge un account online a un account utente in Windows Server Essentials, è possibile gestire entrambi gli account insieme tutto il ciclo di vita di account s.  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a>Comprendere lo stato dell'account online  
 Quando si assegna un account Microsoft Online Services a un account utente, l'indirizzo di posta elettronica per l'account viene visualizzato nel **account online Microsoft** colonna di **utenti** pagina del Dashboard. (In Windows Server Essentials, la colonna è denominata **account Office 365**.)  
  
-   Un'icona blu accanto a un indirizzo di posta elettronica indica che l'account online è attivo. Vale a dire l'account disponga di una licenza di Office 365 corrente e l'utente può usare l'ID online per accedere a Office 365.  
  
-   Un'icona ombreggiata accanto all'indirizzo di posta elettronica indica l'account online è inattivo œ perché la licenza non è più attiva o l'account online è stata annullata. Quando si annulla l'assegnazione di un account online s, la licenza viene rimossa e all'utente viene impedito l'accesso a Office 365 con l'account. Il server viene comunque mantenuto il mapping tra il nome dell'account utente e l'indirizzo di posta elettronica di Office 365.  
  
###  <a name="BKMK_UnassignOnlineAccount"></a>Limitare l'accesso a un account online  
 Cosa fare se un utente lascia l'organizzazione o si desidera limitare l'accesso s utente per i servizi di Office 365? Se si gestione degli account online insieme agli account utente in Windows Server Essentials gli utenti, sono disponibili tre opzioni:  
  
-   **Annullare l'assegnazione dell'account online** ? Se si desidera impedire a un utente di usare Office 365 senza impedire l'accesso alle risorse sul server, è necessario annullare l'assegnazione dell'account online. La licenza di Office 365 verrà rilasciata e l'utente viene impedito l'accesso a Office 365. Il server viene comunque mantenuto il mapping tra il nome dell'account utente e l'indirizzo di posta elettronica di Office 365. Per istruzioni, vedere [per annullare l'assegnazione di un account online da un account utente](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **Disattivare l'account utente** ? Se si disattiva un account utente perché un dipendente lascia, temporaneamente o definitivamente, viene disattivato anche l'account online s. Non può essere utilizzato l'account online, ma i dati utente, inclusa la posta elettronica, saranno conservati in Microsoft Online Services. Per istruzioni, vedere [disattivare un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) in [gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **Rimuovere l'account utente** ? Se si rimuove un account utente, l'account online viene rimosso anche da Microsoft Online Services. Per istruzioni, vedere [rimuovere un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) in [gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  Tenere presente che quando viene rimosso un account online, i dati dell'utente sono soggetto ai dati di criteri di conservazione dei Microsoft Online Services. Se è necessario conservare i dati dell'utente s persona dopo che un dipendente lascia, disattivare l'account utente invece di rimuoverlo.  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a>Per annullare l'assegnazione di un account online da un account utente  
  
1.  Nel Dashboard, fare clic su **utenti**.  
  
2.  Fare doppio clic su account utente nell'elenco, quindi fare clic su **annullare l'assegnazione di un account online Microsoft** (in Windows Server Essentials, fare clic su **annullare l'assegnazione di un account Office 365**).  
  
3.  Al prompt di conferma, fare clic su **Sì**.  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a>Gestire indirizzi di posta elettronica per Exchange Online  
 Mediante l'aggiunta di indirizzi di posta elettronica all'account utente s online in Windows Server Essentials, è possibile consentire all'utente di ricevere messaggi su più indirizzi di posta elettronica di Exchange Online.  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a>Per aggiungere indirizzi di posta elettronica aggiuntive a un utente s account online Microsoft  
  
1.  Nel Dashboard di Windows Server Essentials, fare clic su **utenti**.  
  
2.  Fare doppio clic su account utente nell'elenco, quindi fare clic su **Visualizza proprietà account**.  
  
3.  Nel **Microsoft online** scheda delle proprietà dell'account (o **Office 365** scheda in Windows Server Essentials), fare clic su **Aggiungi**.  
  
4.  Digitare il nuovo alias di posta elettronica e quindi selezionare il dominio di posta elettronica.  
  
5.  Fare clic su **OK** due volte.  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a>Gestire gruppi di distribuzione per Exchange Online (solo Windows Server Essentials)  
 Dopo aver integrato il server Windows Server Essentials con Office 365, puoi creare e gestire gruppi di distribuzione per Exchange Online dal Dashboard di Windows Server Essentials. Ll puoi eseguire questa operazione sul **gruppi di distribuzione** scheda che viene aggiunto al **utenti** pagina. Questa scheda viene visualizzata solo se hai un abbonamento a Exchange Online. Questa funzionalità non è disponibile in Windows Server Essentials.  
  
 Utilizzare le procedure seguenti per:  
  
-   [Aggiungere un gruppo di distribuzione](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Modificare i membri di un gruppo di distribuzione](#BKMK_ChangeGroupMembers)  
  
-   [Modifica appartenenza al gruppo di distribuzione un utente s](#BKMK_EditUserMemberships)  
  
-   [Rimuovere un gruppo di distribuzione](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a>Per aggiungere un gruppo di distribuzione  
  
1.  Nel Dashboard di Windows Server Essentials, fare clic su **utenti**, quindi fare clic su di **gruppi di distribuzione** scheda.  
  
2.  In **attività gruppo di distribuzione**, fare clic su **aggiungere un gruppo di distribuzione**.  
  
     Aggiungi una creazione guidata nuovo gruppo di distribuzione viene visualizzato.  
  
3.  Nel **aggiungere un nuovo gruppo di distribuzione** pagina, immettere le informazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Immettere un nome gruppo, una descrizione facoltativa e alias di posta elettronica per il nuovo gruppo di distribuzione.  
  
    -   Per impostazione predefinita, il gruppo di distribuzione può ricevere posta elettronica da utenti esterni all'organizzazione. Se non vuoi evitarlo, deselezionare questa opzione.  
  
4.  Nel **aggiungere i membri del gruppo** pagina, utilizzare il **Aggiungi** pulsante per aggiungere account utente attivi che hanno un account online assegnato loro e altri gruppi di distribuzione, il nuovo gruppo di distribuzione. Fare clic su **Avanti**.  
  
     Il nuovo gruppo di distribuzione viene creato in Exchange Online.  
  
###  <a name="BKMK_ChangeGroupMembers"></a>Per modificare i membri di un gruppo di distribuzione  
  
1.  Nel Dashboard, fare clic su **utenti**, quindi fare clic su di **gruppi di distribuzione** scheda.  
  
2.  Pulsante destro del mouse sul gruppo di distribuzione nell'elenco, quindi fare clic su **modificare l'appartenenza al gruppo**.  
  
3.  Utilizzare il **Aggiungi** e **rimuovere** pulsanti per aggiungere o rimuovere account online attivi dal gruppo di distribuzione. Fare clic su **Avanti** per aggiornare l'appartenenza al gruppo di distribuzione in Exchange Online.  
  
###  <a name="BKMK_EditUserMemberships"></a>Per modificare l'appartenenza ai gruppi di distribuzione un utente s  
  
1.  Nel Dashboard, fare clic su **utenti**.  
  
2.  Fare doppio clic su account utente nell'elenco, quindi fare clic su **Visualizza proprietà account**.  
  
3.  Nelle proprietà dell'account utente, fare clic su di **gruppi di distribuzione** scheda e quindi fare clic su **modifica**.  
  
4.  Nel **Modifica appartenenza al gruppo**, utilizzare il **Aggiungi** e **rimuovere** pulsanti per aggiungere o rimuovere gruppi di distribuzione dall'account utente e quindi fare clic su **Chiudi**.  
  
5.  Fare clic su **OK** salvare le proprietà dell'account utente aggiornato.  
  
###  <a name="BKMK_RemoveDistributionGroup"></a>Per rimuovere un gruppo di distribuzione  
  
1.  Nel Dashboard, fare clic su **utenti**, quindi fare clic su di **gruppi di distribuzione** scheda.  
  
2.  Pulsante destro del mouse sul gruppo di distribuzione nell'elenco, quindi fare clic su **rimuovere il gruppo**.  
  
3.  Al prompt di conferma, fare clic su **Elimina gruppo**.  
  
     Il gruppo di distribuzione viene rimosso da Exchange Online.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Gestire Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gestire Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
