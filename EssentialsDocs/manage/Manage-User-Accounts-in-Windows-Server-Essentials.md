---
title: Gestire gli account utente in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91175836e4453860b17d2655e6a5a831645de410
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>Gestire gli account utente in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

La pagina utenti del Dashboard di Windows Server Essentials centralizza le informazioni e attività che consentono di gestire gli account utente nella rete delle piccole aziende. Per una panoramica del Dashboard utenti, vedere [Panoramica del Dashboard](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
  
##  <a name="BKMK_ManageAccounts"></a>Gestione degli account utente  
 Gli argomenti seguenti forniscono informazioni su come usare il Dashboard di Windows Server Essentials per gestire gli account utente sul server:  
  
-   [Aggiungere un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [Rimuovere un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [Visualizzare gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [Modificare il nome visualizzato per l'account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [Attivare un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [Disattivare un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [Informazioni sugli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [Gestire gli account utente tramite il Dashboard](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a>Aggiungere un account utente  
 Quando si aggiunge un account utente, l'utente assegnato può accedere alla rete ed è possibile assegnare all'utente l'autorizzazione per accedere alle risorse di rete, ad esempio cartelle condivise e il sito accesso Web remoto. Windows Server Essentials include l'aggiunta guidata Account utente che consente di:  
  
-   Fornire un nome e una password per l'account utente.  
  
-   Definire l'account come amministratore o come utente standard.  
  
-   Selezionare le può accedere all'account utente di cartelle condivise.  
  
-   Specificare se l'account utente disponga di accesso remoto alla rete.  
  
-   Selezionare le opzioni di posta elettronica, se applicabile.  
  
-   Assegnare un account Microsoft Online Services, definito per un account Office 365 in Windows Server Essentials, se applicabile.  
  
-   Assegnare gruppi di utenti (solo Windows Server Essentials).  
  
> [!NOTE]
>  -   Caratteri non ASCII non sono supportati in Microsoft Azure Active Directory (Azure AD). Non utilizzare caratteri non ASCII per la password, se il server è integrato con Azure AD.  
> -   Le opzioni di posta elettronica sono disponibili solo se si installa un componente aggiuntivo che fornisce il servizio di posta elettronica.  
  
##### <a name="to-add-a-user-account"></a>Per aggiungere un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nel **attività utenti** riquadro, fare clic su **aggiungere un account utente**. L'aggiunta guidata Account utente viene visualizzato.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
###  <a name="BKMK_Remove"></a>Rimuovere un account utente  
 Quando si sceglie di rimuovere un account utente dal server, una procedura guidata consente di eliminare l'account selezionato. Per questo motivo, non è possibile utilizzare l'account per accedere alla rete o per accedere a tutte le risorse di rete. In alternativa, è anche possibile eliminare i file per l'account utente nello stesso momento di rimuovere l'account. Se non si desidera rimuovere definitivamente l'account utente, è possibile disattivare l'account utente invece per sospendere l'accesso alle risorse di rete.  
  
> [!IMPORTANT]
>  Se un account utente dispone di una linea account assegnato, quando si rimuove l'account utente, l'account online viene rimosso anche da Microsoft Online Services e i dati s dell'utente, inclusa la posta elettronica, sono soggette a dati di criteri di conservazione in Microsoft Online Services. Se si desidera conservare i dati utente per l'account online, disattivare l'account utente invece di rimuoverlo. Per ulteriori informazioni, vedere [gestire account Online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account"></a>Per rimuovere un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera rimuovere.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **rimuovere l'account utente**. L'eliminazione viene visualizzata una procedura guidata Account utente.  
  
5.  Nel **si desidera conservare i file? ** pagina della procedura guidata, è possibile scegliere di eliminare i file utente s, inclusi i backup di cronologia File e la cartella reindirizzata per l'account utente. Per mantenere l'utente file s, lasciare vuota la casella di controllo. Dopo la selezione, fare clic su **Avanti**.  
  
6.  Fare clic su **eliminare account**.  
  
> [!NOTE]
>  Dopo la rimozione di un account utente, l'account non viene più visualizzato nell'elenco di account utente. Se si sceglie di eliminare i file, il server in modo definitivo la cartella utente s dal **utenti** cartella del server e dal **backup cronologia File** cartella del server.  
>   
>  Se si dispone di un provider di posta elettronica integrato, verrà rimossi anche l'account di posta elettronica assegnato all'account utente.  
  
###  <a name="BKMK_Manage3"></a>Visualizzare gli account utente  
 Il **utenti** sezione del Dashboard di Windows Server Essentials Visualizza un elenco di account utente di rete. L'elenco include anche informazioni aggiuntive su ogni account.  
  
##### <a name="to-view-a-list-of-user-accounts"></a>Per visualizzare un elenco di account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Nella barra di spostamento principale fare clic su **utenti**.  
  
3.  Il Dashboard visualizza un elenco di account utente corrente.  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>Per visualizzare o modificare le proprietà di un account utente  
  
1.  Nell'elenco di account utente, selezionare l'account per cui si desidera visualizzare o modificare le proprietà.  
  
2.  Nel **attività < utente Account\ >** riquadro, fare clic su **Visualizza proprietà account**. Il **proprietà** pagina per l'account utente viene visualizzato.  
  
3.  Fare clic su una scheda per visualizzare le proprietà per l'account specifico.  
  
4.  Per salvare le modifiche apportate alle proprietà dell'account utente, fare clic su **applica**.  
  
###  <a name="BKMK_Manage4"></a>Modificare il nome visualizzato per l'account utente  
 Il nome visualizzato è il nome visualizzato nel **nome** colonna di **utenti** pagina del Dashboard. Modificare il nome visualizzato non modifica il nome di accesso o di accesso per un account utente.  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>Per modificare il nome visualizzato per un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera modificare.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **Visualizza proprietà account**. Il **proprietà** pagina per l'account utente viene visualizzato.  
  
5.  Nel **generale** , digitare un nuovo **nome** e **cognome** per l'account utente e quindi fare clic su **OK**.  
  
     Il nuovo nome viene visualizzato nell'elenco di account utente.  
  
###  <a name="BKMK_Manage5"></a>Attivare un account utente  
 Quando si attiva un account utente, l'utente assegnato può accedere alle risorse di rete accesso e di rete a cui l'account disponga delle autorizzazioni, ad esempio cartelle condivise e il sito accesso Web remoto.  
  
> [!NOTE]
>  È possibile attivare solo un account utente disattivato. È possibile attivare un account utente dopo averlo rimosso dal server.  
  
##### <a name="to-activate-a-user-account"></a>Per attivare un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nella visualizzazione elenco, selezionare l'account utente che si desidera attivare.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **attivare l'account utente**.  
  
5.  Nella finestra di conferma, fare clic su **Sì** per confermare l'azione.  
  
> [!NOTE]
>  Dopo aver attivato un account utente, viene visualizzato lo stato per l'account **Active**. L'account utente otterrà di nuovo gli stessi diritti di accesso assegnati prima della disattivazione di account.  
>   
>  Se si dispone di un provider di posta elettronica integrato, sarà attivato anche l'account di posta elettronica assegnato all'account utente.  
  
###  <a name="BKMK_Manage6"></a>Disattivare un account utente  
 Quando si disattiva un account utente, account di accesso al server è temporaneamente sospesa. Per questo motivo, l'utente assegnato non può utilizzare l'account per accedere alle risorse di rete, ad esempio cartelle condivise o il sito accesso Web remoto fino all'attivazione dell'account.  
  
 Se l'account utente è assegnato un account online Microsoft, viene disattivato anche l'account online. L'utente non può utilizzare le risorse di Office 365 e altri servizi online sottoscritti, ma i dati s dell'utente, inclusa la posta elettronica, saranno conservati in Microsoft Online Services.  
  
> [!NOTE]
>  È possibile disattivare solo un account utente attualmente attivo.  
  
##### <a name="to-deactivate-a-user-account"></a>Per disattivare un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nella visualizzazione elenco, selezionare l'account utente che si desidera disattivare.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **disattivare l'account utente**.  
  
5.  Nella finestra di conferma, fare clic su **Sì** per confermare l'azione.  
  
> [!NOTE]
>  Dopo la disattivazione di un account utente, viene visualizzato lo stato per l'account **inattivo**.  
>   
>  Se si dispone di un provider di posta elettronica integrato, sarà disattivato anche l'account di posta elettronica assegnato all'account utente.  
  
###  <a name="BKMK_Manage7"></a>Informazioni sugli account utente  
 Un account utente fornisce informazioni importanti a Windows Server Essentials, che consente agli utenti di accedere alle informazioni che vengono archiviate nel server e rende possibile per singoli utenti di creare e gestire file e delle impostazioni. Gli utenti possono accedere a qualsiasi computer nella rete se hanno un account utente di Windows Server Essentials e le autorizzazioni necessarie per accedere a un computer. Gli utenti di accedere agli account utente con il nome utente e password.  
  
 Esistono due tipi principali di account utente. Ogni tipo concede agli utenti un diverso livello di controllo del computer:  
  
-   **Standard** gli account sono per uso quotidiano del computer. L'account standard consente di proteggere la rete, impedendo agli utenti di apportare modifiche che influiscono su altri utenti, ad esempio l'eliminazione di file o la modifica delle impostazioni di rete.  
  
-   **Amministratore** account offrono maggiore controllo su una rete di computer. È consigliabile assegnare l'amministratore account di tipo solo quando necessario.  
  
###  <a name="BKMK_Manage8"></a>Gestire gli account utente tramite il Dashboard  
 Windows Server Essentials consente di eseguire attività amministrative comuni tramite il Dashboard di Windows Server Essentials. Per impostazione predefinita, il **utenti** pagina del Dashboard include due schede **utenti** e **gruppi di utenti**.  
  
> [!NOTE]
>  -   Se si integra il server che esegue Windows Server Essentials con Office 365, una nuova scheda denominata **gruppi di distribuzione** aggiunto anche all'interno di **utenti** pagina del Dashboard.  
> -   In Windows Server Essentials, il **utenti** pagina del Dashboard include solo una scheda - **utenti**.  
  
 Il **utenti** scheda include quanto segue:  
  
-   Un elenco di account utente, che visualizza:  
  
    -   Il nome dell'utente.  
  
    -   Il nome di accesso per l'account utente.  
  
    -   Se l'account utente dispone dell'autorizzazione di accesso remoto via Internet. In un punto qualsiasi dell'autorizzazione di accesso per un account utente è **consentito** o **non è consentito**.  
  
    -   Se la cronologia File per questo account utente è gestita dal server che esegue Windows Server Essentials. Lo stato di cronologia File per un account utente è **gestito** o **non gestiti**.  
  
    -   Il livello di accesso assegnato all'account utente. È possibile assegnare **utente Standard** accesso o **amministratore** accesso per un account utente.  
  
    -   Lo stato dell'account utente. Può essere un account utente **Active**, **inattivo**, o **incompleto**.  
  
    -   In Windows Server Essentials, se il server è integrato con Office 365 o Windows Intune, viene visualizzato l'account online Microsoft.  
  
    -   In Windows Server Essentials, se il server è integrato con Microsoft Office 365, viene visualizzato lo stato dell'account Office 365 (noto in Windows Server Essentials come account online Microsoft) per l'account utente.  
  
-   Un riquadro dei dettagli con altre informazioni su un account utente selezionato.  
  
-   Un riquadro attività che include:  
  
    -   Un set di attività amministrative dell'account utente, ad esempio la visualizzazione e la rimozione degli account utente e la modifica delle password.  
  
    -   Attività che consentono di impostare o modificare le impostazioni per tutti gli account utente nella rete globalmente.  
  
 La tabella seguente descrive le varie attività di account utente disponibili la **utenti** scheda. Alcune delle attività sono specifiche di account utente e sono visibili solo quando si seleziona un account utente nell'elenco.  
  
> [!NOTE]
>  Se si integra Office 365 con Windows Server Essentials, saranno rese disponibili attività aggiuntive. Per ulteriori informazioni, vedere [gestire account Online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
### <a name="user-account-tasks-in-the-dashboard"></a>Attività dell'account utente nel Dashboard di  
  
|Nome attività|Descrizione|  
|---------------|-----------------|  
|Visualizzare le proprietà dell'account|Consente di visualizzare e modificare le proprietà dell'account utente selezionato e per specificare le autorizzazioni per l'account di accesso.|  
|Disattivare l'account utente|Un account utente disattivato non possa accedere alle risorse di rete accesso o di rete, ad esempio cartelle condivise o le stampanti.|  
|Attivare l'account utente|Un account utente attivato può accedere alla rete e può accedere alle risorse di rete, come definito dalle autorizzazioni dell'account.|  
|Rimuovere l'account utente|Consente di rimuovere l'account utente selezionato.|  
|Modificare la password dell'account utente|Consente di reimpostare la password di rete per l'account utente selezionato.|  
|Aggiungere un account utente|Avvia l'aggiunta guidata Account utente, che consente di creare un singolo account utente nuovo che dispone di accesso utente standard o accesso administrator.|  
|Assegnare un account online Microsoft|Aggiunge un account online Microsoft per l'account utente di rete locale selezionato.<br /><br /> Questa attività viene visualizzata quando il server è integrato con Microsoft online services, ad esempio Office 365.|  
|Aggiungere gli account online Microsoft|Aggiunge account online Microsoft e li associa agli account utente di rete locale.<br /><br /> Questa attività viene visualizzata quando il server è integrato con Microsoft online services, ad esempio Office 365.|  
|Imposta criteri password|Consente di modificare i valori della password di criteri per la rete.|  
|Importare gli account online Microsoft|Esegue un'importazione in blocco degli account da Microsoft online services alla rete locale.<br /><br /> Questa attività viene visualizzata quando il server è integrato con Microsoft online services, ad esempio Office 365.|  
|Aggiornamento|Aggiorna la scheda utenti.<br /><br /> Questa attività è applicabile a Windows Server Essentials.|  
|Modificare le impostazioni di cronologia File|Consente di modificare le impostazioni di cronologia File, ad esempio frequenza o la durata del backup.<br /><br /> Questa attività è applicabile a Windows Server Essentials.|  
|Esportare tutte le connessioni remote|Crea un. File in formato CSV di tutte le connessioni remote al server che si sono verificati negli ultimi 30 giorni.|  
  
##  <a name="BKMK_ManageAccess"></a>Gestione delle password e accesso  
 Gli argomenti seguenti forniscono informazioni su come usare il Dashboard di Windows Server Essentials per gestire le password degli account utente e l'accesso utente alle cartelle condivise sul server:  
  
-   [Modificare o reimpostare la password per un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [Cosa è necessario conoscere i criteri password](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [Modificare i criteri password](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [Livello di accesso alle cartelle condivise](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [Conservare e gestire l'accesso ai file per gli account utente rimossi](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [Sincronizzare la password modalità ripristino servizi directory con la password dell'amministratore di rete](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [Concedere agli account utente l'autorizzazione per desktop remoto](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [Consentire agli utenti di accedere alle risorse sul server](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [Modificare le autorizzazioni di accesso remoto per un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [Modificare le autorizzazioni di rete privata virtuale per un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [Modificare l'accesso alle cartelle condivise interne per un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [Consente agli account utente di stabilire una sessione desktop remoto al computer](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a>Modificare o reimpostare la password per un account utente  
 Per modificare o reimpostare una password di account utente, segui questi passaggi.  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>Per reimpostare la password per un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera reimpostare.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **modificare la password dell'account utente**. Verrà visualizzata la procedura guidata Password di Account utente modifica.  
  
5.  Digitare una nuova password per l'account utente e quindi digitare nuovamente la password per confermarla.  
  
6.  Fare clic su **Cambia password**.  
  
7.  Fornire la nuova password all'utente.  
  
    > [!IMPORTANT]
    >  -   Non siano in grado di modificare la password, se i criteri password per il tuo account sono stato impostato **password senza scadenza**.  
    > -   Caratteri non ASCII non sono supportati in Azure AD. Pertanto, se il server è integrato con Azure AD, non utilizzare caratteri non ASCII per la password.  
    > -   Se l'utente è assegnato un account online Microsoft (noto in Windows Server Essentials come un account Office 365), la password viene sincronizzata con la password dell'account online. L'utente utilizzerà la nuova password per accedere al server o accedere a Office 365. Per ulteriori informazioni, vedere [gestire account Online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
###  <a name="BKMK_Access3"></a>Cosa è necessario conoscere i criteri password  
 I criteri password sono un set di regole che definiscono come gli utenti di creare e utilizzano password. I criteri aiutano a evitare accessi non autorizzati ai dati utente e altre informazioni archiviate sul server. I criteri password viene applicato a tutti gli account utente che accedono alla rete.  
  
 I criteri password di Windows Server Essentials costituito da tre elementi principali come indicato di seguito:  
  
-   **Lunghezza della password**.  È più una password, più sicuro. Le password vuote non sono sicure.  
  
-   **Complessità della password**.  Le password complesse includono una combinazione di lettere maiuscole e minuscole (a-z, A-Z), di base (0-9) numeri e simboli non alfabetici (quali!, @, #, _,-). Le password complesse sono meno suscettibili all'accesso non autorizzato. Le password che contengono i nomi utente, i compleanni o altre informazioni personali non offrono una sicurezza adeguata.  
  
-   **La validità della password**.  Windows Server Essentials richiede che gli utenti di modificare la password almeno una volta ogni 180 giorni. In alternativa, è possibile scegliere di usare password senza scadenza.  
  
 Per rendere più semplice implementare un criterio password nella rete di computer, Windows Server Essentials offre un semplice strumento che consente di impostare o modificare i criteri password in uno dei profili di quattro criteri predefiniti seguenti:  
  
-   **Debole**.  Gli utenti possono specificare qualsiasi password non vuota.  
  
-   **Media**.  Queste password devono contenere almeno 5 caratteri. Non è necessaria una password complessa.  
  
-   **Mediamente complessa**.  Queste password devono contenere almeno 5 caratteri e devono includere lettere, numeri e simboli.  
  
-   **Alta**.  Queste password devono contenere almeno 7 caratteri e devono includere lettere, numeri e simboli. Queste password sono più sicure, ma potrebbero essere più difficili da ricordare.  
  
    > [!NOTE]
    >  Le password non possono contenere l'indirizzo di posta elettronica o di nome utente.  
    >   
    >  Se si integra con Office 365, l'integrazione impone il **sicuro** criteri per le password e aggiorna i criteri per includere i seguenti requisiti:  
    >   
    >  -   Le password devono contenere 8 a 16 caratteri.  
    > -   Le password non possono contenere uno spazio o l'alias di posta elettronica di Office 365.  
  
 Per impostazione predefinita, l'installazione del server imposta i criteri password predefiniti il **sicuro** opzione.  
  
###  <a name="BKMK_Access4"></a>Modificare i criteri password  
 Utilizzare la procedura seguente per impostare o modificare i criteri password in uno qualsiasi dei quattro profili i criteri predefiniti.  
  
##### <a name="to-change-the-password-policy"></a>Per modificare i criteri password  
  
1.  Aprire il Dashboard di Windows Server Essentials, quindi fare clic su **utenti**.  
  
2.  Nel **attività utenti** riquadro, fare clic su **Imposta criteri password**.  
  
3.  Nel **modificare i criteri Password** schermata, impostare il livello di complessità della password spostando il dispositivo di scorrimento.  
  
     Microsoft consiglia di impostare la complessità della password **sicuro**.  
  
    > [!NOTE]
    >  In alternativa, è inoltre possibile selezionare **password senza scadenza**. Questa impostazione è meno sicura, e pertanto non è consigliata.  
  
4.  Fare clic su **modificare criteri**.  
  
###  <a name="BKMK_Access5"></a>Livello di accesso alle cartelle condivise  
 Come procedura consigliata, è necessario assegnare le autorizzazioni più restrittive che tuttavia permettono comunque agli utenti di eseguire le attività necessarie.  
  
 Sono disponibili tre impostazioni di accesso per le cartelle condivise sul server:  
  
-   **Lettura/scrittura**.  Selezionare questa impostazione se si desidera consentire l'autorizzazione dell'account utente creare, modificare ed eliminare i file nella cartella condivisa.  
  
-   **Sola lettura**.  Selezionare questa impostazione se si desidera consentire l'autorizzazione dell'account utente solo di leggere i file nella cartella condivisa. Gli account utente con accesso in sola lettura non possono creare, modificare o eliminare i file nella cartella condivisa.  
  
-   **Nessun accesso**.  Selezionare questa impostazione se non si desidera l'account utente acceda ai file disponibili nella cartella condivisa.  
  
###  <a name="BKMK_Access6"></a>Conservare e gestire l'accesso ai file per gli account utente rimossi  
 L'amministratore di rete può rimuovere un account utente e scegliere di mantenere l'utente file s per l'utilizzo futuro. In questo scenario, l'account utente rimosso non può più essere utilizzato per accedere alla rete. Tuttavia, i file per l'utente verranno salvati in una cartella condivisa, che può essere condivisa con un altro utente.  
  
> [!IMPORTANT]
>  Tenere presente che se si rimuove un account utente che dispone di un account online Microsoft assegnato, viene rimosso anche l'account online e i dati utente, inclusa la posta elettronica, sono soggette a criteri di conservazione dei dati in Microsoft Online Services. Per conservare i dati dell'utente per l'account online, disattivare l'account utente invece di rimuoverlo. Per ulteriori informazioni, vedere [gestire account Online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-user-s-files"></a>Per rimuovere un account utente ma conservare l'accesso ai file utente s  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera rimuovere.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **rimuovere l'account utente**. L'eliminazione viene visualizzata una procedura guidata Account utente.  
  
5.  Nel **si desidera conservare i file? ** pagina, assicurarsi che il **Elimina i file inclusi backup di cronologia File e cartella reindirizzata per questo account utente** controllo casella è deselezionata, quindi fare clic su **Avanti**.  
  
     Verrà visualizzata una pagina di conferma che informa che sta eliminando l'account, ma i file saranno conservati.  
  
6.  Fare clic su **eliminare account** per rimuovere l'account utente.  
  
 Dopo la rimozione dell'account utente, l'amministratore può concedere a un altro account utente l'accesso alla cartella condivisa.  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>Per assegnare un account utente l'autorizzazione per accedere a una cartella condivisa  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **archiviazione**, quindi fare clic su di **le cartelle del Server** scheda.  
  
3.  Nell'elenco delle cartelle, selezionare il **utenti** cartella.  
  
4.  Nel **attività utenti** riquadro, fare clic su **aprire la cartella**. Apre Esplora risorse e visualizza il contenuto del **utenti** cartella.  
  
5.  Fare clic sulla cartella per l'account utente che si desidera condividere e quindi fare clic su **proprietà**.  
  
6.  In **< utente Account\ > proprietà**, fare clic su di **condivisione** scheda e quindi fare clic su **condivisione**.  
  
7.  Nel **la condivisione di File** finestra dei comandi, digitare o selezionare il nome dell'account utente con cui si vuole condividere la cartella, quindi fare clic su **Aggiungi**.  
  
8.  Scegliere il **livello di autorizzazione** che si desidera che l'account utente, quindi fare clic su **condivisione**.  
  
###  <a name="BKMK_Access7"></a>Sincronizzare la password modalità ripristino servizi directory con la password dell'amministratore di rete  
 Modalità ripristino servizi directory (DSRM) è una modalità di avvio speciale per il ripristino o il ripristino di Active Directory. Il sistema operativo Usa modalità ripristino servizi directory per accedere al computer se Active Directory non funziona o deve essere ripristinato. Se la password di amministratore di rete e la password modalità ripristino servizi directory sono diversi, modalità ripristino servizi directory non verrà caricato.  
  
 Durante un'installazione pulita, prima di Windows Server Essentials, il programma imposta la password modalità ripristino servizi directory per la password dell'account amministratore rete specificate durante l'installazione o nel file di risposte della migrazione. Quando si modifica la password di amministratore di rete (come consigliato in genere ogni 60 giorni per la sicurezza del server maggiore), la modifica della password non sarà inoltrata alla modalità ripristino servizi directory. Ciò produce una password non corrispondenti. In questo caso, è possibile utilizzare le seguenti soluzioni per manualmente o automaticamente sincronizzare la password s amministratore di rete con la password modalità ripristino servizi directory.  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Per sincronizzare manualmente la password modalità ripristino servizi directory con un account di amministratore di rete  
  
1.  Al prompt dei comandi, eseguire `ntdsutil.exe` per aprire lo strumento ntdsutil.  
  
2.  Per reimpostare la password modalità ripristino servizi directory, digitare **set dsrm password**.  
  
3.  Per sincronizzare la password modalità ripristino servizi directory in un controller di dominio con l'account s amministratore di rete corrente, digitare:  
  
     **sincronizzazione dall'account di dominio** *< account_amministratore_rete_corrente >*, quindi premere INVIO.  
  
 Poiché sarà modificata periodicamente la password per l'account di amministratore di rete, per garantire che la password DSRM è sempre la stessa password attuale dell'amministratore di rete, è consigliabile creare automaticamente un'attività pianificata per sincronizzare la password della modalità ripristino servizi directory per la password dell'amministratore di rete ogni giorno.  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Per sincronizzare automaticamente la password modalità ripristino servizi directory con un account di amministratore di rete  
  
1.  Dal server, aprire **strumenti di amministrazione**e quindi fare doppio clic su **utilità di pianificazione**.  
  
2.  Nell'utilità di pianificazione **azioni** riquadro, fare clic su **Crea attività**.  
  
3.  Nel **nome** casella di testo, digitare un nome per l'attività, ad esempio **DSRM con Sincraut**e quindi seleziona il **Esegui con privilegi più elevati** opzione.  
  
4.  Definire quando eseguire l'attività:  
  
    1.  Nel **Crea attività** la finestra di dialogo, fare clic sul **trigger** scheda e quindi fare clic su **New**.  
  
    2.  Nel **nuovo Trigger** la finestra di dialogo, selezionare l'opzione relativa alla ricorrenza, specificare l'intervallo di ricorrenza e scegliere un'ora di inizio.  
  
        > [!NOTE]
        >  Come procedura consigliata, è necessario impostare l'attività da eseguire ogni giorno durante le ore di inattività.  
  
    3.  Fare clic su **OK** per salvare le modifiche e tornare al **Crea attività** la finestra di dialogo.  
  
5.  Definire le azioni dell'attività:  
  
    1.  Fare clic su di **azioni** scheda e quindi fare clic su **New**. Il **nuova azione** viene visualizzata la finestra di dialogo.  
  
    2.  Nel **azione** elenco, fare clic su **avviare un programma**, quindi passare a **C:\WINDOWS\SYSTEM32\ntdsutil.exe**.  
  
    3.  Nel **aggiungere argomenti**testo (facoltativo) digitare il comando seguente (è necessario includere le virgolette): **impostare sincronizzazione password modalità ripristino servizi directory dall'account di dominio SBS_network_administrator_account q q** dove *SBS_network_administrator_account* è il nome dell'account amministratore s rete corrente.  
  
6.  Fare clic su **OK** due volte per salvare l'attività e chiudere il **Crea attività** la finestra di dialogo. La nuova attività viene visualizzata nel **attività attive** sezione **pianificazione**.  
  
###  <a name="BKMK_Access8"></a>Concedere agli account utente l'autorizzazione per desktop remoto  
 Nell'installazione predefinita di Windows Server Essentials, gli utenti di rete non si dispongano dell'autorizzazione per stabilire una connessione remota a computer o altre risorse di rete.  
  
 Prima che gli utenti di rete possono stabilire una connessione remota alle risorse di rete, è necessario configurare accesso remoto via Internet. Dopo aver impostato l'accesso remoto via Internet, gli utenti possono accedere a file, applicazioni e i computer nella rete aziendale da un dispositivo in qualsiasi posizione con una connessione Internet.  
  
 La configurazione guidata accesso remoto via Internet consente di abilitare due metodi di accesso remoto:  
  
-   Rete privata virtuale (VPN)  
  
-   Accesso Web remoto  
  
 Quando si esegue la procedura guidata, è anche possibile consentire l'accesso remoto via Internet per tutti gli account utente attuali e appena aggiunti.  
  
 Per configurare accesso remoto via Internet, aprire il Dashboard **Home** pagina, fare clic su **installazione**, quindi fare clic su **configurare accesso remoto via Internet**.  
  
 Per ulteriori informazioni sull'accesso remoto via Internet, vedere [Gestione accesso remoto via Internet](Manage-Anywhere-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_Access9"></a>Consentire agli utenti di accedere alle risorse sul server  
  In questa sezione si riferisce a un server che esegue Windows Server Essentials o Windows Server Essentials, oppure un server che esegue Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato.  
  
 Se si desidera agli utenti di utilizzare l'accesso remoto e/o singoli account utente, al termine della connessione di un computer al server, è possibile creare rete nuovo account utente per gli utenti del computer in rete sul server tramite il Dashboard. Per ulteriori informazioni sulla creazione di un account utente, vedere [aggiungere un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1). Dopo aver creato gli account utente, è necessario fornire le informazioni di nome e una password utente rete per gli utenti del computer client in modo che possano accedere alle risorse del server tramite la finestra di avvio.  
  
 Per ogni account utente creato è possibile impostare le proprietà dell'account di accesso alle risorse seguenti tramite l'utente:  
  
-   **Cartelle condivise**.  Per impostazione predefinita, gli amministratori di rete hanno **lettura/scrittura** dispone dell'autorizzazione per tutte le cartelle condivise e gli account utente standard **Read-only** le autorizzazioni per la cartella società. Se i flussi multimediali sono abilitati, è possibile assegnare le autorizzazioni di accesso di cartelle per singoli account utente standard per le cartelle condivise seguenti: **musica**, **immagini**, **registrazioni TV**, e **video**. È possibile impostare le autorizzazioni per gli account utente accedere alle cartelle condivise nel **cartelle condivise** scheda delle proprietà dell'account utente.  
  
-   **Accesso remoto via Internet**.  Per impostazione predefinita, gli amministratori di rete possono utilizzare per accedere alle risorse di server VPN o accesso Web remoto. Per gli account utente standard, è necessario impostare autorizzazioni dell'account utente nel **accesso remoto via Internet** scheda.  
  
-   **Accesso al computer**.  Per impostazione predefinita, gli amministratori di rete possono accedere a tutti i computer nella rete. Per gli account utente standard, tuttavia, è possibile impostare autorizzazioni dell'account utente per l'accesso ai computer della rete sul **accesso Computer** scheda delle proprietà dell'account utente.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>Per modificare le proprietà di account utente in Windows Server Essentials 2012 R2  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera modificare.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **Visualizza proprietà account**.  
  
5.  Nel **< utente Account\ > proprietà**, eseguire le operazioni seguenti:  
  
    1.  Nel **cartelle condivise** scheda, impostare le autorizzazioni della cartella appropriata per ogni cartella condivisa in base alle esigenze.  
  
    2.  Nel **accesso remoto via Internet** scheda:  
  
        1.  Per consentire all'utente di connettersi al server tramite la VPN, selezionare il **Consenti privata rete virtuale (VPN)** casella di controllo.  
  
        2.  Per consentire all'utente di connettersi al server tramite accesso Web remoto, selezionare il **Consenti accesso Web remoto e Accedi ai servizi Web** casella di controllo.  
  
    3.  Nel **accesso Computer** scheda, selezionare i computer di rete che vuoi avere accesso all'utente.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>Per modificare le proprietà di account utente in Windows Server 2012 Essentials  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera modificare.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **proprietà**.  
  
5.  Nel **< utente Account\ > proprietà**, eseguire le operazioni seguenti:  
  
    1.  Nel **generale** selezionare **utente può visualizzare gli avvisi di integrità** se l'account utente deve accedere ai rapporti di integrità di rete.  
  
    2.  Nel **cartelle condivise** scheda, impostare le autorizzazioni della cartella appropriata per ogni cartella condivisa in base alle esigenze.  
  
    3.  Nel **accesso remoto via Internet** scheda:  
  
        1.  Per consentire all'utente di connettersi al server tramite la VPN, selezionare il **Consenti privata rete virtuale (VPN)** casella di controllo.  
  
        2.  Per consentire all'utente di connettersi al server tramite accesso Web remoto, selezionare il **Consenti accesso Web remoto e Accedi ai servizi Web** casella di controllo.  
  
    4.  Nel **accesso Computer** scheda, selezionare i computer di rete che vuoi avere accesso all'utente.  
  
###  <a name="BKMK_Access10"></a>Modificare le autorizzazioni di accesso remoto per un account utente  
 Un utente può accedere alle risorse del server da una posizione remota tramite una rete privata virtuale (VPN), accesso Web remoto o altri servizi Web. Per impostazione predefinita, le autorizzazioni di accesso remoto sono attivate per gli utenti di rete quando si configura accesso remoto via Internet in Windows Server Essentials tramite il Dashboard.  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>Per modificare le autorizzazioni di accesso remoto per un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera modificare.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **Visualizza proprietà account**. Il **proprietà** pagina per l'account utente viene visualizzato.  
  
5.  Nel **accesso remoto via Internet** scheda, eseguire le operazioni seguenti:  
  
    -   Selezionare il **Consenti privata rete virtuale (VPN)** casella di controllo per consentire all'utente di connettersi al server tramite VPN.  
  
    -   Selezionare il **Consenti accesso Web remoto e Accedi ai servizi Web** casella di controllo per consentire all'utente di connettersi al server tramite accesso Web remoto.  
  
6.  Fare clic su **applica**, quindi fare clic su **OK**.  
  
###  <a name="BKMK_Access11"></a>Modificare le autorizzazioni di rete privata virtuale per un account utente  
 È possibile utilizzare una rete privata virtuale (VPN) per connettersi a Windows Server Essentials e accedere a tutte le risorse che sono archiviate nel server. Ciò è particolarmente utile se si dispone di un computer client che viene configurato con account di rete che può essere utilizzato per connettersi a un server Windows Server Essentials ospitato tramite una connessione VPN. Tutti gli account utente appena creati nel server Windows Server Essentials ospitato devono usare VPN per accedere al computer client per la prima volta.  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>Per modificare le autorizzazioni della VPN per gli utenti di rete  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente a cui si vogliono concedere le autorizzazioni per accedere al desktop in remoto.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **proprietà**.  
  
5.  Nel **< utente Account\ > proprietà**, fare clic su di **accesso remoto via Internet** scheda.  
  
6.  Nel **accesso remoto via Internet** scheda, per consentire all'utente di connettersi al server tramite la VPN, selezionare il **Consenti privata rete virtuale (VPN)** casella di controllo.  
  
7.  Fare clic su **applica**, quindi fare clic su **OK**.  
  
###  <a name="BKMK_Access12"></a>Modificare l'accesso alle cartelle condivise interne per un account utente  
 È possibile gestire l'accesso alle cartelle condivise sul server tramite le attività nel **cartelle Server** scheda del Dashboard. Per impostazione predefinita, vengono create le cartelle del server seguenti quando si installa Windows Server Essentials:  
  
-   **Backup Computer client**.  Usato per archiviare i backup dei computer client creati da Windows Server backup. Questa cartella del server non è condivisa.  
  
-   **Società**.  Usato per archiviare e accedere ai documenti correlati all'organizzazione dagli utenti di rete.  
  
-   **Backup cronologia file**.  Per impostazione predefinita, Windows Server Essentials vengono archiviati i backup di file creati con cronologia File. Questa cartella del server non è condivisa.  
  
-   **Il reindirizzamento delle cartelle**.  Usato per archiviare e accedere alle cartelle configurate per il reindirizzamento cartelle per gli utenti della rete. Questa cartella del server non è condivisa.  
  
-   **Musica**.  Usato per archiviare e accedere ai file musicali da parte degli utenti di rete. In questa cartella viene creata quando si attiva la condivisione di file multimediali.  
  
-   **Immagini**.  Permette l'archiviazione e l'accesso alle immagini da utenti della rete. In questa cartella viene creata quando si attiva la condivisione di file multimediali.  
  
-   **Registrazioni**.  Usato per archiviare e accedere ai programmi TV registrati per gli utenti della rete. In questa cartella viene creata quando si attiva la condivisione di file multimediali.  
  
-   **Video**.  Usato per archiviare e accedere ai video da parte degli utenti di rete. In questa cartella viene creata quando si attiva la condivisione di file multimediali.  
  
-   **Gli utenti**.  Usato per archiviare e accedere ai file da parte degli utenti di rete. Una cartella specifiche dell'utente viene generata automaticamente nel **utenti** cartella del server per ogni account utente di rete che crei.  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>Per modificare l'accesso a una cartella condivisa per un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Fare clic su **archiviazione**, quindi fare clic su **cartelle Server**.  
  
3.  Individuare e selezionare la cartella del server per cui si desidera modificare le autorizzazioni.  
  
4.  Nel riquadro attività fare clic su **visualizzare le proprietà della cartella**.  
  
5.  In **< FolderName\ > proprietà**, fare clic su **condivisione**, selezionare il livello di accesso utente appropriato per gli account utente elencati e quindi fare clic su **applica**.  
  
    > [!NOTE]
    >  È possibile modificare le autorizzazioni di condivisione per **backup cronologia File**, **il reindirizzamento delle cartelle**, e **utenti** le cartelle del server. Di conseguenza, le proprietà della cartella di queste cartelle del server non includono un **condivisione** scheda.  
  
###  <a name="BKMK_Access13"></a>Consente agli account utente di stabilire una sessione desktop remoto al computer  
  In questa sezione si riferisce a un server che esegue Windows Server Essentials o Windows Server Essentials, oppure un server che esegue Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato.  
  
 L'amministratore di rete può concedere le autorizzazioni agli utenti di rete che consentono loro di accedere ai computer di rete da una posizione remota.  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>Per consentire agli utenti di accedere ai computer di rete da una posizione remota  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera concedere le autorizzazioni per accedere al desktop in remoto.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **proprietà**.  
  
5.  Nel **< utente Account\ > proprietà**, fare clic su di **accesso Computer** scheda.  
  
6.  Selezionare i computer che si desidera essere in grado di accedere in remoto, quindi fare clic su questo account utente **OK**.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire gli account Online per gli utenti](Manage-Online-Accounts-for-Users.md)  
  
-   [Connessione](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
