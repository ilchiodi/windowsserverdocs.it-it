---
title: Gestire gli account online per gli utenti di Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: dc3170c7d5267eef6f339dc229b1b9daaf9ac9ec
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980276"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Gestire gli account online per gli utenti di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si integra il server di Windows Server Essentials con Microsoft Office 365, è possibile gestire gli account online insieme agli account utente dal dashboard. In questo argomento si scopriranno gli elementi che è possibile ottenere gestendo gli account di Microsoft Online Services dal dashboard, come creare e gestire account online dal dashboard e come gestire gli indirizzi di posta elettronica e i gruppi di distribuzione per Exchange Online dal dashboard.  

  
> [!NOTE]
>  Per gestire gli account di Microsoft Online Services in Windows Server Essentials, è necessario integrare il server con Office 365. Per istruzioni, vedere [gestire Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Se si gestiscono gli account online in Windows Server Essentials, si è abituati a vedere gli account di Microsoft Online Services indicati come *account di Office 365*. Nel dashboard di Windows Server Essentials le etichette sono state modificate in *account di Microsoft Online Services*o gli *account Microsoft Online* per brevità. Sono cambiate solo le etichette, mentre gli account e le procedure sono rimasti invariati. Nella maggior parte delle procedure in questo argomento viene usato il termine *account online*.  
  
## <a name="in-this-topic"></a>Contenuto dell'argomento  
  
-   [Perché è necessario gestire gli account online dal dashboard?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Creare account online](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Gestisci account online](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Gestire gli indirizzi di posta elettronica per Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Gestire i gruppi di distribuzione per Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a>Perché è necessario gestire gli account online dal dashboard?  
 Quando si usa il dashboard per assegnare un account di Microsoft Online Services a un account utente, le password degli account vengono sincronizzate automaticamente ed è possibile mantenere insieme i due account in tutto il ciclo di vita dell'account utente.  
  
 È utile per l'utente, che può usare la stessa password per accedere alle risorse sul server e in Office 365. È anche possibile applicare gli stessi requisiti di password per l'accesso alle risorse di Office 365 necessarie per le risorse interne.  
  
### <a name="how-does-password-synchronization-work"></a>Funzionamento della sincronizzazione delle password  
 Quando si usa il dashboard per assegnare un account di Microsoft Online Services a un account utente, la password dell'account utente viene sincronizzata automaticamente con l'account online dell'utente. Questo significa che un utente deve avere solo una password per accedere alle risorse sul server e in Office 365. Inoltre, è possibile usare lo stesso nome per l'account utente e per l'ID online dell'utente.  
  
 Le password vengono sincronizzate immediatamente e automaticamente quando un utente cambia la password per l'account utente da un computer appartenente a un dominio o mediante Accesso Web remoto.  
  
> [!IMPORTANT]
>  Se Office 365 è integrato con Windows Server Essentials, gli utenti non devono modificare la password per l'account Microsoft Online dal portale di Office 365. Tale operazione, infatti, interrompe la sincronizzazione delle password.  
  
### <a name="simplified-account-creation"></a>Creazione di account semplificata  
 È disponibile un altro vantaggio quando si creano gli account online iniziali dal dashboard. È possibile creare account online per tutti gli utenti contemporaneamente. D'altra parte, se i dipendenti usano già Office 365 e si configura un nuovo server di Windows Server Essentials, è possibile creare tutti gli account utente dagli account online con una singola azione. Per altre informazioni, vedere [Creare account online](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Gestire indirizzi di posta elettronica e gruppi di distribuzione dal dashboard  
 È possibile gestire gli indirizzi di posta elettronica e i gruppi di distribuzione per Exchange Online dal dashboard. È possibile usare il dominio Internet dell'organizzazione negli indirizzi di posta elettronica. È possibile eseguire tutte queste operazioni dal dashboard, senza accedere a Office 365. Per gestire i gruppi di distribuzione dal dashboard, è necessario usare Windows Server Essentials. Questa funzionalità non è supportata in Windows Server Essentials. Per altre informazioni, vedere [Gestire indirizzi di posta elettronica per Exchange Online](#BKMK_SECTION_ManageEmailAddresses) e [Gestire gruppi di distribuzione per Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Gestire account utente e account online insieme  
 È possibile gestire un account online insieme all'account utente per tutto il ciclo di vita dell'account. Se si disattiva l'account utente, viene disattivato anche l'account online in Microsoft Online Services. Se si rimuove un account utente, viene rimosso anche l'account online. Per altre informazioni, vedere [Gestire account online](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a>Creare account online  
 Dopo aver integrato il server con Office 365, è possibile creare account di Microsoft Online Services per gli utenti dal dashboard. La creazione degli account online è molto flessibile. Se si dispone di una nuova sottoscrizione di Office 365, è possibile creare in blocco gli account online per tutti gli utenti. Se gli account online sono già stati creati in Office 365, non preoccuparti. Se si configura un nuovo server, è possibile creare gli account utente sul server importando gli account online. È possibile assegnare un account online nuovo o esistente quando si crea un singolo account utente o quando si aggiunge un account online a un account utente esistente.  
  
 **Requisiti di licenza** Sarà necessaria una licenza utente per ogni account online creato. Controllare la pagina **Office 365** nel dashboard per visualizzare il numero di licenze utente disponibili tramite l'abbonamento a Office 365. Se è necessario aggiungere altre licenze utente, sarà possibile aprire la sottoscrizione di Office 365 in Office 365 a tale scopo.  
  
 **Completamento per gli utenti** Dopo aver aggiunto un account online per un account utente, al successivo accesso l'utente dovrà modificare la password per il proprio account utente. La nuova password viene sincronizzata immediatamente con l'account online. Successivamente, è possibile usare la password per accedere a Office 365 con l'ID online.  
  
> [!IMPORTANT]
>  Evidenziare agli utenti che non dovrebbero mai modificare la password dell'account online in Office 365. La password verrà cambiata automaticamente ogni volta che viene cambiata la password dell'account utente. Se cambiano la password online in Office 365, la sincronizzazione delle password verrà interruppe.  
  
 Usare le procedure in questa sezione per:  
  
-   [Creare in blocco gli account online per gli account utente esistenti](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importare gli account utente dagli account di Microsoft Online Services](#BKMK_ToImportUserAccounts)  
  
-   [Creare un nuovo account utente con un account online assegnato](#BKMK_ToCreateaNewUserAccount)  
  
-   [Assegnare un account online a un account utente](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Se si usa Windows Server Essentials, viene visualizzato l' *account di Office 365* anziché l' *account Microsoft Online Services* per tutte queste procedure. Il processo è lo stesso, ma la terminologia è cambiata in Windows Server Essentials.  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a>Per creare in blocco gli account online per gli account utente esistenti  
  
1.  Accedere al server come amministratore e aprire il dashboard di Windows Server Essentials.  
  
2.  Nel dashboard aprire la pagina **Utenti**.  
  
3.  In **Attività Utenti** fare clic su **Aggiungi account di Microsoft Online**.  
  
     Nella pagina **Aggiungi account di Microsoft Online Services** della procedura guidata vengono visualizzati tutti gli account utente a cui non è associato un account online Microsoft. Per impostazione predefinita sono selezionati tutti gli account e viene suggerito il nome utente per l'ID online Microsoft. Se è stato collegato un dominio Internet personalizzato alla sottoscrizione di Office 365, tale dominio verrà usato per impostazione predefinita.  
  
4.  Nella pagina **Aggiungi account di Microsoft Online Services** esaminare gli account che verranno creati. Ad esempio, cercare gli utenti che dispongono già di un account online con un ID online diverso e assicurarsi che sia selezionato il dominio che si vuole usare negli indirizzi di posta elettronica. Dopo avere apportato tutte le modifiche necessarie, fare clic su **Avanti**.  
  
5.  Nella pagina **assegna licenze di Microsoft Online Services** selezionare i servizi di Office 365 che gli utenti useranno. Quando si fa clic su **Avanti** viene avviata la creazione degli account.  
  
    > [!NOTE]
    >  È necessario avere una licenza del servizio in Office 365 per ogni account online. È possibile controllare le licenze disponibili e, se necessario, accedere all'abbonamento per aggiungere le licenze dalla pagina **Office 365** nel dashboard.  
  
6.  Comunicare agli utenti che ora dispongono di un account online Microsoft. Devono modificare la password dell'account utente di rete prima di poter accedere a Office 365. Per istruzioni, vedere [Per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a>Per iniziare a usare un nuovo account online Microsoft  
  
1.  Accedere al computer con il proprio account utente di rete.  
  
2.  Cambiare la password dell'account utente. Per iniziare, premere CTRL+ALT+CANC, quindi fare clic su **Cambia password**.  
  
     Quando si cambia la password, questa viene sincronizzata con il nuovo account online. È ora possibile usare la stessa password per accedere a Office 365.  
  
3.  [Accedere a Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) usando il nuovo ID online e la password dell'account utente.  
  
    > [!IMPORTANT]
    >  Non modificare la password dell'account online in Office 365. In questo modo, infatti, si interrompe la sincronizzazione delle password. La password online verrà aggiornata ogni volta che si cambia la password dell'account utente di rete.  
  
###  <a name="BKMK_ToImportUserAccounts"></a>Per importare gli account utente dagli account online esistenti  
  
1.  Nel dashboard aprire la pagina **Utenti**.  
  
2.  In **Attività Utenti**fare clic su **Importa account da Office 365**.  
  
     Nella pagina successiva vengono visualizzati tutti gli account online per la sottoscrizione di Office 365 che non dispongono di un account utente sul server. Per impostazione predefinita sono selezionati tutti gli account e viene suggerito l'ID online per il nome utente.  
  
3.  Per creare gli account utente:  
  
    1.  Apportare le modifiche necessarie agli account utente proposti.  
  
    2.  Facoltativamente, fare clic sul collegamento per visualizzare le password temporanee che verranno assegnate agli account utente. Sarà necessario fornire agli utenti la password temporanea insieme al nuovo nome account.  
  
         Dopo aver creato gli account, è possibile trovare le password elencate in questo file: *SystemDrive*\Users\\*amminoffice365*nuovoutenteserver. txt dove amminoffice365 è l'account di rete usato per amministrare Office 365 nel server e nuovoutenteserver è il nuovo\\ nome dell'account utente.  
  
    3.  Fare clic su **Avanti** per creare gli account utente.  
  
4.  Consentire agli utenti di conoscerne i nuovi account utente e le password temporanee che useranno per accedere al server per la prima volta e dovranno modificare la password dopo l'accesso. Per istruzioni per gli utenti, vedere [Per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Assicurarsi che le password per l'account online verranno sincronizzate con l'account utente in futuro e non devono modificare la password online in Office 365.  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a>Per creare un nuovo account utente con un account online assegnato  
  
1.  Nel dashboard fare clic su **Utenti**.  
  
2.  In **Attività Utenti**fare clic su **Aggiungi account utente**. Verrà visualizzata la procedura guidata Aggiungi account utente.  
  
3.  Seguire le istruzioni per creare l'account utente.  
  
4.  Nella pagina **Assegna un account di Microsoft Online Services** creare un nuovo account online per l'utente o assegnare un account online esistente:  
  
    -   Per creare un nuovo account online, fare clic su **Crea un nuovo account di Microsoft Online Services e assegnalo a questo account utente**e digitare un nome per l'account di Microsoft Online Services (per impostazione predefinita viene usato il nome dell'utente per l'ID online). Scegliere quindi **Avanti**.  
  
    -   Per assegnare un account online Microsoft esistente, fare clic su **Assegna un account di Microsoft Online Services esistente a questo account utente**, quindi selezionare un account dall'elenco a discesa. Scegliere quindi **Avanti**.  
  
    > [!NOTE]
    >  In Windows Server Essentials gli account di Microsoft Online Services sono denominati account di Office 365 nelle procedure guidate e nelle etichette del dashboard.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
6.  Comunicare all'utente che dovrà cambiare la password dell'account utente per poter accedere a Office 365 con il nuovo account online. Per istruzioni, vedere [Per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>Per assegnare un account online a un account utente  
  
1.  Nel dashboard fare clic su **Utenti**.  
  
2.  Fare clic con il pulsante destro del mouse sull'account utente nell'elenco e scegliere **Assegna un account di Microsoft Online**. Verrà visualizzata la procedura guidata Assegna un account di Microsoft Online Services.  
  
3.  Assegnare un account online esistente o crearne uno nuovo per l'utente. L'ID online predefinito per un nuovo account è il nome utente. Fare clic su **Avanti** per aggiungere l'account online all'account utente.  
  
4.  Esaminare le informazioni nell'ultima pagina della procedura guidata, quindi fare clic su **Chiudi**.  
  
5.  Comunicare all'utente che dovrà cambiare la password dell'account utente per poter accedere a Office 365 con il nuovo account online. Per istruzioni, vedere [Per iniziare a usare un nuovo account online Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a>Gestisci account online  
 Quando si aggiunge un account online a un account utente in Windows Server Essentials, è possibile gestire entrambi gli account in tutto il ciclo di vita dell'account.  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a>Informazioni sullo stato dell'account online  
 Quando si assegna un account di Microsoft Online Services a un account utente, l'indirizzo di posta elettronica per l'account viene visualizzato nella colonna **Account online Microsoft** nella pagina **Utenti** del dashboard (In Windows Server Essentials, l'etichetta di colonna è l' **account di Office 365**).  
  
-   Un'icona blu accanto a un indirizzo di posta elettronica indica che l'account online è attivo. Ovvero, l'account ha una licenza di Office 365 e l'utente può usare l'ID online per accedere a Office 365.  
  
-   Un'icona ombreggiata accanto all'indirizzo di posta elettronica indica che l'account online non è attivo, perché la licenza non è più attiva o l'account online non è stato assegnato. Quando si annulla l'assegnazione di un account online di un utente, la licenza viene rimossa e all'utente viene impedito l'accesso a Office 365 con l'account. Tuttavia, il server gestisce il mapping tra il nome dell'account utente e l'indirizzo di posta elettronica di Office 365.  
  
###  <a name="BKMK_UnassignOnlineAccount"></a>Limita l'accesso a un account online  
 Cosa fare se un utente lascia l'organizzazione o si vuole limitare l'accesso degli utenti ai servizi di Office 365? Se si gestiscono gli account online degli utenti insieme agli account utente in Windows Server Essentials, sono disponibili tre opzioni:  
  
-   **Annullare l'assegnazione dell'account online** ? Se si vuole impedire a un utente di usare Office 365 senza impedire l'accesso alle risorse sul server, è necessario annullare l'assegnazione dell'account online. La licenza di Office 365 verrà rilasciata e all'utente viene impedito l'accesso a Office 365. Tuttavia, il server gestisce il mapping tra il nome dell'account utente e l'indirizzo di posta elettronica di Office 365. Per istruzioni, vedere [per annullare l'assegnazione di un account online da un account utente](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **Disattivare l'account utente** ? Se si disattiva un account utente perché un dipendente lascia, temporaneamente o definitivamente, anche l'account online dell'utente viene disattivato. L'account online non può essere usato, ma i dati dell'utente, inclusa la posta elettronica, vengono mantenuti in Microsoft Online Services. Per istruzioni, vedere [disattivare un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) in [gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **Rimuovere l'account utente** ? Se si rimuove un account utente, viene rimosso anche l'account online da Microsoft Online Services. Per istruzioni, vedere [rimuovere un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) in [gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  Tenere presente che quando si rimuove un account online i dati dell'utente sono soggetti ai criteri di conservazione dei dati di Microsoft Online Services. Se è necessario conservare i dati utente della persona dopo che un dipendente ha lasciato, disattivare l'account utente anziché rimuoverlo.  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a>Per annullare l'assegnazione di un account online a un account utente  
  
1.  Nel dashboard fare clic su **Utenti**.  
  
2.  Fare clic con il pulsante destro del mouse sull'account utente nell'elenco e quindi scegliere **Annulla l'assegnazione di un account online Microsoft** (in Windows Server Essentials, fare clic su **Annulla l'assegnazione di un account Office 365**).  
  
3.  Alla richiesta di conferma fare clic su **Sì**.  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a>Gestire gli indirizzi di posta elettronica per Exchange Online  
 Aggiungendo gli indirizzi di posta elettronica all'account online dell'utente in Windows Server Essentials, è possibile consentire all'utente di ricevere messaggi di posta elettronica in più indirizzi di posta elettronica in Exchange Online.  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a>Per aggiungere altri indirizzi di posta elettronica all'account online Microsoft di un utente  
  
1.  Nel dashboard di Windows Server Essentials fare clic su **utenti**.  
  
2.  Fare clic con il pulsante destro del mouse sull'account utente nell'elenco, quindi scegliere **Visualizza proprietà account**.  
  
3.  Nella scheda **Microsoft Online** delle proprietà dell'account (o nella scheda **Office 365** in Windows Server Essentials) fare clic su **Aggiungi**.  
  
4.  Digitare il nuovo alias di posta elettronica e selezionare il dominio di posta elettronica.  
  
5.  Fare clic su **OK** due volte.  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a>Gestire i gruppi di distribuzione per Exchange Online (solo Windows Server Essentials)  
 Dopo aver integrato il server di Windows Server Essentials con Office 365, è possibile creare e gestire gruppi di distribuzione per Exchange Online dal dashboard di Windows Server Essentials. Questa operazione viene eseguita nella scheda **gruppi di distribuzione** che viene aggiunta alla pagina **utenti** . Questa scheda viene visualizzata solo se si dispone di un abbonamento a Exchange Online. Questa funzionalità non è disponibile in Windows Server Essentials.  
  
 Usare le procedure seguenti per:  
  
-   [Aggiungere un gruppo di distribuzione](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Modificare i membri di un gruppo di distribuzione](#BKMK_ChangeGroupMembers)  
  
-   [Modificare le appartenenze ai gruppi di distribuzione di un utente](#BKMK_EditUserMemberships)  
  
-   [Rimuovere un gruppo di distribuzione](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a>Per aggiungere un gruppo di distribuzione  
  
1.  Nel dashboard di Windows Server Essentials fare clic su **utenti**e quindi sulla scheda **gruppi di distribuzione** .  
  
2.  In **Attività Gruppo di distribuzione**fare clic su **Aggiungi gruppo di distribuzione**.  
  
     Verrà visualizzata la procedura guidata Aggiungi nuovo gruppo di distribuzione.  
  
3.  Nel **Aggiungi nuovo gruppo di distribuzione** immettere quanto segue e fare clic su **Avanti**:  
  
    -   Immettere un nome di gruppo, una descrizione facoltativa e un alias di posta elettronica per il nuovo gruppo di distribuzione.  
  
    -   Per impostazione predefinita, il gruppo di distribuzione può ricevere messaggi di posta elettronica da utenti esterni all'organizzazione. Per evitarlo, deselezionare questa opzione.  
  
4.  Nella pagina **Aggiungi membri al gruppo** fare clic su **Aggiungi** per aggiungere account utente attivi a cui è assegnato un account online, e altri gruppi di distribuzione, al nuovo gruppo di distribuzione. Scegliere quindi **Avanti**.  
  
     Il nuovo gruppo di distribuzione viene creato in Exchange Online.  
  
###  <a name="BKMK_ChangeGroupMembers"></a>Per modificare i membri di un gruppo di distribuzione  
  
1.  Nel dashboard fare clic su **Utenti**, quindi sulla scheda **Gruppi di distribuzione** .  
  
2.  Fare clic con il pulsante destro del mouse sul gruppo di distribuzione nell'elenco, quindi scegliere **Modifica appartenenza al gruppo**.  
  
3.  Usare i pulsanti **Aggiungi** e **Rimuovi** per aggiungere o rimuovere account online attivi dal gruppo di distribuzione. Fare quindi clic su **Avanti** per aggiornare l'appartenenza al gruppo di distribuzione in Exchange Online.  
  
###  <a name="BKMK_EditUserMemberships"></a>Per modificare l'appartenenza a un gruppo di distribuzione di un utente  
  
1.  Nel dashboard fare clic su **Utenti**.  
  
2.  Fare clic con il pulsante destro del mouse sull'account utente nell'elenco, quindi scegliere **Visualizza proprietà account**.  
  
3.  Nelle proprietà dell'account utente fare clic sulla scheda **Gruppi di distribuzione**, quindi fare clic su **Modifica**.  
  
4.  Nella casella **Modifica appartenenza a gruppi** usare i pulsanti **Aggiungi** e **Rimuovi** per aggiungere o rimuovere gruppi di distribuzione dall'account utente, quindi fare clic su **Chiudi**.  
  
5.  Fare clic su **OK** per salvare le proprietà aggiornate dell'account utente.  
  
###  <a name="BKMK_RemoveDistributionGroup"></a>Per rimuovere un gruppo di distribuzione  
  
1.  Nel dashboard fare clic su **Utenti**, quindi sulla scheda **Gruppi di distribuzione** .  
  
2.  Fare clic con il pulsante destro del mouse sul gruppo di distribuzione nell'elenco, quindi scegliere **Rimuovi gruppo**.  
  
3.  Alla richiesta di conferma fare clic su **Elimina gruppo**.  
  
     Il gruppo di distribuzione viene rimosso da Exchange Online.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Gestire Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gestire i Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
