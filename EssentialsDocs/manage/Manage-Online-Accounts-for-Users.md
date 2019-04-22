---
title: Gestire gli account online per gli utenti di Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824092"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Gestire gli account online per gli utenti di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si integra il server Windows Server Essentials con Microsoft Office 365, è possibile gestire gli account online insieme agli account utente dal Dashboard. In questo argomento, si ll scoprire i vantaggi gestendo gli utenti account Microsoft Online Services dal Dashboard, come creare e gestire gli account online dal Dashboard e come gestire indirizzi di posta elettronica e i gruppi di distribuzione per Exchange Online dal Dashboard.  

  
> [!NOTE]
>  Per gestire gli account Microsoft Online Services in Windows Server Essentials, è necessario integrare il server con Office 365. Per istruzioni, vedere [gestire Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Se si gestiscono gli account online in Windows Server Essentials, è abituati a vedere gli account Microsoft Online Services detti *account di Office 365*. Nel Dashboard in Windows Server Essentials, le etichette sono state modificate in *account di Microsoft Online Services*, o *gli account online Microsoft* breve. Sono cambiate solo le etichette, mentre gli account e le procedure sono rimasti invariati. Nella maggior parte delle procedure in questo argomento viene usato il termine *account online*.  
  
## <a name="in-this-topic"></a>Contenuto dell'argomento  
  
-   [Il motivo per cui gestire gli account online dal Dashboard?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Creare account online](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Gestire gli account online](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Gestire indirizzi di posta elettronica per Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Gestire i gruppi di distribuzione per Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a> Il motivo per cui gestire gli account online dal Dashboard?  
 Quando si usa il Dashboard per assegnare un account Microsoft Online Services a un account utente, verranno sincronizzati automaticamente le password degli account, ed è possibile mantenere i due account tra loro in tutto il ciclo di vita del s di account utente.  
  
 Lo s utile per l'utente, che è possibile usare la stessa password per accedere alle risorse sul server e in Office 365. Ed è possibile applicare gli stessi requisiti di password per l'accesso alle risorse di Office 365 necessarie per le risorse interne.  
  
### <a name="how-does-password-synchronization-work"></a>Funzionamento della sincronizzazione delle password  
 Quando si usa il Dashboard per assegnare un account Microsoft Online Services a un account utente, la password dell'account utente viene sincronizzata automaticamente con l'account online s utente. Ciò significa che un utente basterà una sola password per accedere alle risorse sul server e in Office 365. Inoltre, è possibile usare lo stesso nome per l'account utente e l'ID utente s online.  
  
 Le password vengono sincronizzate immediatamente e automaticamente quando un utente cambia la password per l'account utente da un computer appartenente a un dominio o mediante Accesso Web remoto.  
  
> [!IMPORTANT]
>  Se l'integrazione di Office 365 con Windows Server Essentials, gli utenti non devono cambiare la password per l'account online Microsoft dal portale di Office 365. Tale operazione, infatti, interrompe la sincronizzazione delle password.  
  
### <a name="simplified-account-creation"></a>Creazione di account semplificata  
 Lì s un altro vantaggio quando si creano gli account online iniziali dal Dashboard. È possibile creare account online per tutti gli utenti contemporaneamente. D'altra parte, se i dipendenti usano già Office 365 e si sta impostando un nuovo server Windows Server Essentials, è possibile creare tutti gli account utente dagli account online con un'unica azione. Per altre informazioni, vedere [Creare account online](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Gestire indirizzi di posta elettronica e gruppi di distribuzione dal dashboard  
 È possibile gestire gli indirizzi di posta elettronica e i gruppi di distribuzione per Exchange Online dal dashboard. Ed è possibile usare il dominio Internet s dell'organizzazione negli indirizzi di posta elettronica. È possibile eseguire tutte queste dal Dashboard, senza accedere a Office 365. (È necessario usare Windows Server Essentials per gestire i gruppi di distribuzione dal Dashboard. Questa funzionalità non è supportata in Windows Server Essentials.) Per altre informazioni, vedere [Gestire indirizzi di posta elettronica per Exchange Online](#BKMK_SECTION_ManageEmailAddresses) e [Gestire gruppi di distribuzione per Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Gestire account utente e account online insieme  
 Ed è possibile gestire un account online insieme all'account utente in tutto il ciclo di vita di account s. Se si disattiva l'account utente, viene disattivato anche l'account online in Microsoft Online Services. Se si rimuove un account utente, viene rimosso anche l'account online. Per altre informazioni, vedere [Gestire account online](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a> Creare account online  
 Dopo aver integrato il server con Office 365, è s per Microsoft Online Services creare comodamente account per gli utenti dal Dashboard. È necessario una notevole flessibilità nella creazione degli account online. Se si dispone di una nuova sottoscrizione di Office 365, è possibile creare in blocco gli account online per tutti gli utenti. Se è già stato creato gli account online in Office 365, si vediamo come funziona. Se si sta impostando un nuovo server, è possibile creare gli account utente sul server importando gli account online. Ed è possibile assegnare un nuovo repository o un account online esistente quando si crea un singolo account utente o quando si aggiunge un account online a un account utente esistente.  
  
 **Requisiti relativi alle licenze** è necessaria una licenza utente per ogni account online creato. Verificare i **Office 365** pagina nel Dashboard per visualizzare il numero di licenze utente disponibile tramite la sottoscrizione di Office 365. Se è necessario aggiungere altre licenze utente, essere in grado di aprire l'abbonamento a Office 365 in Office 365 a tale scopo è ll.  
  
 **Operazioni successive per gli utenti** dopo aver aggiunto un account online per un account utente, l'utente dovrà cambiare la password per l'account utente al successivo accesso. La nuova password viene sincronizzata immediatamente con l'account online. Successivamente, è possibile usare la password per accedere a Office 365 con proprio ID online.  
  
> [!IMPORTANT]
>  Comunicare agli utenti che non si deve mai cambiare la password dell'account online in Office 365. La password verrà cambiata automaticamente ogni volta che viene cambiata la password dell'account utente. Se si modifica la password online in Office 365, la sincronizzazione delle password sarà interrotta.  
  
 Usare le procedure in questa sezione per:  
  
-   [Creare account online per tutti gli utenti in blocco](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importare account utente dagli account Microsoft Online Services](#BKMK_ToImportUserAccounts)  
  
-   [Creare un nuovo account utente con un account online assegnato](#BKMK_ToCreateaNewUserAccount)  
  
-   [Assegnare un account online a un account utente](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Se si usa Windows Server Essentials, si noterà *account di Office 365* invece di *account Microsoft Online Services* durante queste procedure. Il processo è lo stesso, ma la terminologia è cambiata in Windows Server Essentials.  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a> Per creare account online per tutti gli utenti in blocco  
  
1.  Accedere al server come amministratore e aprire il Dashboard di Windows Server Essentials.  
  
2.  Nel dashboard aprire la pagina **Utenti**.  
  
3.  In **Attività Utenti** fare clic su **Aggiungi account di Microsoft Online**.  
  
     Nella pagina **Aggiungi account di Microsoft Online Services** della procedura guidata vengono visualizzati tutti gli account utente a cui non è associato un account online Microsoft. Per impostazione predefinita sono selezionati tutti gli account e viene suggerito il nome utente per l'ID online Microsoft. Se è stato collegato un dominio Internet personalizzato all'abbonamento a Office 365, tale dominio verrà usato per impostazione predefinita.  
  
4.  Nella pagina **Aggiungi account di Microsoft Online Services** esaminare gli account che verranno creati. Ad esempio, cercare gli utenti che dispongono già di un account online con un ID online diverso e assicurarsi che sia selezionato il dominio che si vuole usare negli indirizzi di posta elettronica. Dopo avere apportato tutte le modifiche necessarie, fare clic su **Avanti**.  
  
5.  Nel **licenze di servizi Online Microsoft assegnare** pagina, selezionare i servizi di Office 365 verrà usato dagli utenti. Quando si fa clic su **Avanti** viene avviata la creazione degli account.  
  
    > [!NOTE]
    >  È necessario disporre una licenza di servizio di Office 365 per ogni account online. È possibile controllare le licenze disponibili e, se necessario, accedere all'abbonamento per aggiungere le licenze dalla pagina **Office 365** nel dashboard.  
  
6.  Comunicare agli utenti che ora dispongono di un account online Microsoft. Devono cambiare la password di account utente di rete possono accedere a Office 365. Per istruzioni, vedere [Per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a> Per iniziare a usare un nuovo account online Microsoft  
  
1.  Accedere al computer con il proprio account utente di rete.  
  
2.  Cambiare la password dell'account utente. Per iniziare, premere CTRL+ALT+CANC, quindi fare clic su **Cambia password**.  
  
     Quando si cambia la password, questa viene sincronizzata con il nuovo account online. È ora possibile usare la stessa password per accedere a Office 365.  
  
3.  [Accedere a Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) usando il nuovo ID online e la password dell'account utente.  
  
    > [!IMPORTANT]
    >  Non modificare la password dell'account online in Office 365. In questo modo, infatti, si interrompe la sincronizzazione delle password. La password online verrà aggiornata ogni volta che si cambia la password dell'account utente di rete.  
  
###  <a name="BKMK_ToImportUserAccounts"></a> Per importare account utente dagli account online esistenti  
  
1.  Nel dashboard aprire la pagina **Utenti**.  
  
2.  In **Attività Utenti**fare clic su **Importa account da Office 365**.  
  
     Nella pagina successiva mostra tutti gli account online per la sottoscrizione di Office 365 che non è un account utente nel server. Per impostazione predefinita sono selezionati tutti gli account e viene suggerito l'ID online per il nome utente.  
  
3.  Per creare gli account utente:  
  
    1.  Apportare le modifiche necessarie agli account utente proposti.  
  
    2.  Facoltativamente, fare clic sul collegamento per visualizzare le password temporanee che verranno assegnate agli account utente. Sarà necessario fornire agli utenti la password temporanea insieme al nuovo nome account.  
  
         (Dopo aver creato gli account, si fornite queste password elencate in questo file: *UnitàSistema*\Users\\*Office365admin*\\*Nuovoutenteserver*. txt dove *Office365admin* è l'account di rete utilizzato per l'amministrazione di Office 365 nel server e *Nuovoutenteserver* è il nuovo nome dell'account utente.)  
  
    3.  Fare clic su **Avanti** per creare gli account utente.  
  
4.  Informare gli utenti i nuovi account utente e le password temporanee che useranno per accedere il server per il primo œ tempo e che dovranno cambiare la password dopo l'accesso. Per istruzioni per gli utenti, vedere [Per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Assicurarsi che sappiano che la password per l'account online verrà sincronizzata con il proprio account utente in futuro e gli utenti non devono cambiare la password online in Office 365.  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a> Per creare un nuovo account utente con un account online assegnato  
  
1.  Nel dashboard fare clic su **Utenti**.  
  
2.  In **Attività Utenti**fare clic su **Aggiungi account utente**. Verrà visualizzata la procedura guidata Aggiungi account utente.  
  
3.  Seguire le istruzioni per creare l'account utente.  
  
4.  Nella pagina **Assegna un account di Microsoft Online Services** creare un nuovo account online per l'utente o assegnare un account online esistente:  
  
    -   Per creare un nuovo account online, fare clic su **Crea un nuovo account di Microsoft Online Services e assegnalo a questo account utente**e digitare un nome per l'account di Microsoft Online Services (per impostazione predefinita viene usato il nome dell'utente per l'ID online). Fare quindi clic su **Avanti**.  
  
    -   Per assegnare un account online Microsoft esistente, fare clic su **Assegna un account di Microsoft Online Services esistente a questo account utente**, quindi selezionare un account dall'elenco a discesa. Fare quindi clic su **Avanti**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, account Microsoft Online Services sono indicati come account di Office 365 nelle procedure guidate e nelle etichette del Dashboard.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
6.  Comunicare all'utente che dovrà cambiare la password dell'account utente per poter accedere a Office 365 con il nuovo account online. Per istruzioni, vedere [Per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>Per assegnare un account online a un account utente  
  
1.  Nel dashboard fare clic su **Utenti**.  
  
2.  Fare clic con il pulsante destro del mouse sull'account utente nell'elenco e scegliere **Assegna un account di Microsoft Online**. Verrà visualizzata la procedura guidata Assegna un account di Microsoft Online Services.  
  
3.  Assegnare un account online esistente o crearne uno nuovo per l'utente. L'ID online predefinito per un nuovo account è il nome utente. Fare clic su **Avanti** per aggiungere l'account online all'account utente.  
  
4.  Esaminare le informazioni nell'ultima pagina della procedura guidata, quindi fare clic su **Chiudi**.  
  
5.  Comunicare all'utente che dovrà cambiare la password dell'account utente per poter accedere a Office 365 con il nuovo account online. Per istruzioni, vedere [Per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a> Gestire gli account online  
 Quando si aggiunge un account online a un account utente in Windows Server Essentials, è possibile gestire insieme entrambi gli account in tutto il ciclo di vita di account s.  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a> Comprendere lo stato dell'account online  
 Quando si assegna un account di Microsoft Online Services a un account utente, l'indirizzo di posta elettronica per l'account viene visualizzato nella colonna **Account online Microsoft** nella pagina **Utenti** del dashboard (In Windows Server Essentials, la colonna è denominata **account di Office 365**.)  
  
-   Un'icona blu accanto a un indirizzo di posta elettronica indica che l'account online è attivo. Vale a dire l'account abbia una licenza di Office 365 corrente e l'utente può usare l'ID online per accedere a Office 365.  
  
-   Un'icona ombreggiata accanto all'indirizzo di posta elettronica indica l'account online è inattivo œ perché la licenza non è più attiva o l'account online è stata annullata. Quando si annulla l'assegnazione di un account utente di online s, la licenza viene rimossa e all'utente viene impedito l'accesso a Office 365 usando l'account. Tuttavia, il server gestisce il mapping tra il nome dell'account utente e l'indirizzo di posta elettronica di Office 365.  
  
###  <a name="BKMK_UnassignOnlineAccount"></a> Limitare l'accesso a un account online  
 Cosa fare se un utente lascia l'organizzazione o si vuole limitare l'accesso dell'utente s per i servizi di Office 365? Se si gestione degli account online insieme agli account utente in Windows Server Essentials gli utenti, sono disponibili tre opzioni:  
  
-   **Annullare l'assegnazione dell'account online** ? Se si desidera impedire a un utente di usare Office 365 senza impedire l'accesso alle risorse sul server, è necessario annullare l'assegnazione dell'account online. La licenza di Office 365 verrà rilasciata e l'utente viene impedito l'accesso a Office 365. Tuttavia, il server gestisce il mapping tra il nome dell'account utente e l'indirizzo di posta elettronica di Office 365. Per istruzioni, vedere [annullare l'assegnazione di un account online da un account utente](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **Disattivare l'account utente** ? Se si disattiva un account utente perché un dipendente lascia l'azienda, temporaneamente o definitivamente, viene disattivato anche l'account online s utente. L'account online non può essere usato, ma i dati dell'utente, inclusa la posta elettronica, vengono mantenuti in Microsoft Online Services. Per istruzioni, vedere [disattivare un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) nelle [Manage User Accounts](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **Rimuovere l'account utente** ? Se si rimuove un account utente, l'account online viene rimosso anche da Microsoft Online Services. Per istruzioni, vedere [rimuove un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) nelle [Manage User Accounts](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  Tenere presente che quando si rimuove un account online i dati dell'utente sono soggetti ai criteri di conservazione dei dati di Microsoft Online Services. Se è necessario conservare i dati dell'utente s persona dopo che un dipendente lascia l'azienda, disattivare l'account utente invece di rimuoverlo.  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a> Annullare l'assegnazione di un account online da un account utente  
  
1.  Nel dashboard fare clic su **Utenti**.  
  
2.  Fare doppio clic su account utente nell'elenco e quindi fare clic su **annullare l'assegnazione di un account online Microsoft** (in Windows Server Essentials, fare clic su **annullare l'assegnazione di un account Office 365**).  
  
3.  Alla richiesta di conferma fare clic su **Sì**.  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a> Gestire indirizzi di posta elettronica per Exchange Online  
 Quando si aggiungono indirizzi di posta elettronica all'account utente s online in Windows Server Essentials, è possibile consentire all'utente di ricevere tramite posta elettronica su più indirizzi di posta elettronica di Exchange Online.  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a> Per aggiungere gli indirizzi di posta elettronica aggiuntivi a un utente s account online Microsoft  
  
1.  Nel Dashboard di Windows Server Essentials, fare clic su **utenti**.  
  
2.  Fare clic con il pulsante destro del mouse sull'account utente nell'elenco, quindi scegliere **Visualizza proprietà account**.  
  
3.  Nel **Microsoft online** della scheda delle proprietà dell'account (o il **Office 365** scheda in Windows Server Essentials), fare clic su **Aggiungi**.  
  
4.  Digitare il nuovo alias di posta elettronica e selezionare il dominio di posta elettronica.  
  
5.  Fare clic su **OK** due volte.  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a> Gestire i gruppi di distribuzione per Exchange Online (solo Windows Server Essentials)  
 Dopo aver integrato il server Windows Server Essentials con Office 365, è possibile creare e gestire gruppi di distribuzione per Exchange Online dal Dashboard di Windows Server Essentials. Ll è eseguire questa operazione nel **gruppi di distribuzione** scheda in cui viene aggiunto al **utenti** pagina. Questa scheda viene visualizzata solo se si dispone di un abbonamento a Exchange Online. Questa funzionalità non è disponibile in Windows Server Essentials.  
  
 Usare le procedure seguenti per:  
  
-   [Aggiungere un gruppo di distribuzione](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Modificare i membri di un gruppo di distribuzione](#BKMK_ChangeGroupMembers)  
  
-   [Modifica appartenenza al gruppo un utente s distribuzione](#BKMK_EditUserMemberships)  
  
-   [Rimuovere un gruppo di distribuzione](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a> Per aggiungere un gruppo di distribuzione  
  
1.  Nel Dashboard in Windows Server Essentials, fare clic su **gli utenti**, quindi fare clic sui **gruppi di distribuzione** scheda.  
  
2.  In **Attività Gruppo di distribuzione**fare clic su **Aggiungi gruppo di distribuzione**.  
  
     Verrà visualizzata la procedura guidata Aggiungi nuovo gruppo di distribuzione.  
  
3.  Nel **Aggiungi nuovo gruppo di distribuzione** immettere quanto segue e fare clic su **Avanti**:  
  
    -   Immettere un nome di gruppo, una descrizione facoltativa e un alias di posta elettronica per il nuovo gruppo di distribuzione.  
  
    -   Per impostazione predefinita, il gruppo di distribuzione può ricevere messaggi di posta elettronica da utenti esterni all'organizzazione. Per evitarlo, deselezionare questa opzione.  
  
4.  Nella pagina **Aggiungi membri al gruppo** fare clic su **Aggiungi** per aggiungere account utente attivi a cui è assegnato un account online, e altri gruppi di distribuzione, al nuovo gruppo di distribuzione. Fare quindi clic su **Avanti**.  
  
     Il nuovo gruppo di distribuzione viene creato in Exchange Online.  
  
###  <a name="BKMK_ChangeGroupMembers"></a> Per modificare i membri di un gruppo di distribuzione  
  
1.  Nel dashboard fare clic su **Utenti**, quindi sulla scheda **Gruppi di distribuzione** .  
  
2.  Fare clic con il pulsante destro del mouse sul gruppo di distribuzione nell'elenco, quindi scegliere **Modifica appartenenza al gruppo**.  
  
3.  Usare i pulsanti **Aggiungi** e **Rimuovi** per aggiungere o rimuovere account online attivi dal gruppo di distribuzione. Fare quindi clic su **Avanti** per aggiornare l'appartenenza al gruppo di distribuzione in Exchange Online.  
  
###  <a name="BKMK_EditUserMemberships"></a> Per modificare l'appartenenza ai gruppi di distribuzione un utente s  
  
1.  Nel dashboard fare clic su **Utenti**.  
  
2.  Fare clic con il pulsante destro del mouse sull'account utente nell'elenco, quindi scegliere **Visualizza proprietà account**.  
  
3.  Nelle proprietà dell'account utente fare clic sulla scheda **Gruppi di distribuzione**, quindi fare clic su **Modifica**.  
  
4.  Nella casella **Modifica appartenenza a gruppi** usare i pulsanti **Aggiungi** e **Rimuovi** per aggiungere o rimuovere gruppi di distribuzione dall'account utente, quindi fare clic su **Chiudi**.  
  
5.  Fare clic su **OK** per salvare le proprietà aggiornate dell'account utente.  
  
###  <a name="BKMK_RemoveDistributionGroup"></a> Per rimuovere un gruppo di distribuzione  
  
1.  Nel dashboard fare clic su **Utenti**, quindi sulla scheda **Gruppi di distribuzione** .  
  
2.  Fare clic con il pulsante destro del mouse sul gruppo di distribuzione nell'elenco, quindi scegliere **Rimuovi gruppo**.  
  
3.  Alla richiesta di conferma fare clic su **Elimina gruppo**.  
  
     Il gruppo di distribuzione viene rimosso da Exchange Online.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Gestire Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gestire Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
