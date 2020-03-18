---
title: Gestire gli account utente in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
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
ms.openlocfilehash: 069cbbdf499ce86586390b1031b6fea71f4f2b2a
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322223"
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>Gestire gli account utente in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

La pagina Utenti del Dashboard di Windows Server Essentials centralizza le informazioni e le attività per semplificare la gestione degli account utente nella rete delle piccole aziende. Per una panoramica del dashboard utenti, vedere [Cenni preliminari sul dashboard](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
  
##  <a name="BKMK_ManageAccounts"></a>Gestione degli account utente  
 Gli argomenti seguenti includono informazioni su come usare il Dashboard di Windows Server Essentials per gestire gli account utente sul server:  
  
-   [Aggiungere un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [Rimuovere un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [Visualizzare gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [Modificare il nome visualizzato per l'account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [Attivare un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [Disattivare un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [Informazioni sugli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [Gestire gli account utente tramite il dashboard](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a>Aggiungere un account utente  
 Quando si aggiunge un account utente, l'utente assegnato può accedere alla rete ed è possibile concedere all'utente l'autorizzazione per accedere alle risorse di rete, ad esempio le cartelle condivise e il sito di Accesso Web remoto. Windows Server Essentials include la procedura guidata Aggiungi account utente, che permette di eseguire le operazioni seguenti:  
  
-   Specificare un nome e una password per l'account utente.  
  
-   Definire l'account come amministratore o utente standard.  
  
-   Selezionare le cartelle condivise a cui l'account utente potrà accedere.  
  
-   Specificare se l'account utente dispone di accesso remoto alla rete.  
  
-   Selezionare le opzioni per la posta elettronica, se applicabili.  
  
-   Assegnare un account di Microsoft Online Services (definito account Office 365 in Windows Server Essentials) se applicabile.  
  
-   Assegnare gruppi di utenti (solo Windows Server Essentials).  
  
> [!NOTE]
> - I caratteri non ASCII non sono supportati in Microsoft Azure Active Directory (Azure AD). Non usare caratteri non ASCII nella password, se il server è integrato con Azure AD.  
>   -   Le opzioni relative alla posta elettronica sono disponibili solo se si installa un componente aggiuntivo che offre il servizio di posta elettronica.  
  
##### <a name="to-add-a-user-account"></a>Per aggiungere un account utente  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nel riquadro  **Attività utente** fare clic su **Aggiungi account utente**. Verrà visualizzata la procedura guidata Aggiungi account utente.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
###  <a name="BKMK_Remove"></a>Rimuovere un account utente  
 Quando si sceglie di rimuovere un account utente dal server, l'account selezionato sarà eliminato da una procedura guidata. Non sarà quindi più possibile usare l'account per accedere alla rete o a eventuali risorse di rete. Se si vuole, è anche possibile eliminare i file per l'account utente contemporaneamente alla rimozione dell'account. Se non si vuole rimuovere in modo definitivo un account utente, è possibile disattivarlo, in modo da sospendere l'accesso alle risorse di rete.  
  
> [!IMPORTANT]
>  Se a un account utente è assegnato un account online Microsoft, quando si rimuove l'account utente, anche l'account online viene rimosso da Microsoft Online Services e i dati dell'utente, inclusa la posta elettronica, sono soggetti ai criteri di conservazione dei dati in Microsoft Online Services. Per conservare i dati utente per l'account online, disattivare l'account utente invece di rimuoverlo. Per altre informazioni, vedere [gestire gli account online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account"></a>Per rimuovere un account utente  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nell'elenco di account utente selezionare l'account utente da rimuovere.  
  
4.  Nel riquadro **attività < account utente\>** , fare clic su **Rimuovi account utente**. Sarà visualizzata la procedura guidata Eliminazione account utente.  
  
5.  Nella pagina si **desidera tenere i file?** della procedura guidata è possibile scegliere di eliminare i file dell'utente, inclusi i backup di cronologia file e la cartella reindirizzata per l'account utente. Per mantenere i file dell'utente, lasciare vuota la casella di controllo. Dopo avere effettuato la selezione, fare clic su **Avanti**.  
  
6.  Fare clic su **Elimina account**.  
  
> [!NOTE]
>  Dopo la rimozione di un account utente, l'account non sarà più visualizzato nell'elenco di account utente. Se si sceglie di eliminare i file, il server Elimina definitivamente la cartella dell'utente dalla cartella del server **utenti** e dalla cartella del server **backup cronologia file** .  
>   
>  Se è disponibile un provider di posta elettronica integrato, sarà rimosso anche l'account di posta elettronica assegnato all'account utente.  
  
###  <a name="BKMK_Manage3"></a>Visualizzare gli account utente  
 Nella sezione **Utenti** del Dashboard di Windows Server Essentials è visualizzato un elenco di account utente di rete. L'elenco include anche informazioni aggiuntive su ogni account.  
  
##### <a name="to-view-a-list-of-user-accounts"></a>Per visualizzare un elenco di account utente  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento principale fare clic su **Utenti**.  
  
3.  Nel dashboard sarà visualizzato l'elenco corrente di account utente.  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>Per visualizzare o modificare le proprietà per un account utente  
  
1.  Nell'elenco di account utente selezionare l'account per cui si vogliono visualizzare o modificare le proprietà.  
  
2.  Nel riquadro **attività < account utente\>** fare clic su **Visualizza proprietà account**. Sarà visualizzata la pagina **Proprietà** per l'account utente.  
  
3.  Fare clic su una scheda per visualizzare le proprietà per l'account specifico.  
  
4.  Per salvare eventuali modifiche apportate alle proprietà dell'account utente, fare clic su **Applica**.  
  
###  <a name="BKMK_Manage4"></a>Modificare il nome visualizzato per l'account utente  
 Il nome visualizzato è il nome che compare nella colonna **Nome** della pagina **Utenti** del dashboard. La modifica del nome visualizzato non comporta la modifica del nome di accesso per l'account utente.  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>Per modificare il nome visualizzato per un account utente  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nell'elenco di account utente selezionare quello da modificare.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Visualizza proprietà account**. Sarà visualizzata la pagina **Proprietà** per l'account utente.  
  
5.  Nella scheda **Generale** digitare un nuovo **Nome** e **Cognome** per l'account utente, quindi fare clic su **OK**.  
  
     Il nuovo nome visualizzato sarà incluso nell'elenco di account utente.  
  
###  <a name="BKMK_Manage5"></a>Attivare un account utente  
 Quando si attiva un account utente, l'utente assegnato può accedere alla rete e alle risorse di rete per cui l'account dispone di autorizzazioni, ad esempio le cartelle condivise e il sito di Accesso Web remoto.  
  
> [!NOTE]
>  È possibile attivare solo un account utente disattivato. Non si può attivare un account utente dopo averlo rimosso dal server.  
  
##### <a name="to-activate-a-user-account"></a>Per attivare un account utente  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nella visualizzazione elenco selezionare l'account utente da attivare.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Attiva account utente**.  
  
5.  Nella finestra di conferma fare clic su **Sì** per confermare l'azione.  
  
> [!NOTE]
>  Dopo l'attivazione di un account utente, lo stato dell'account sarà **Attivo**. L'account utente otterrà di nuovo gli stessi diritti di accesso assegnati prima della disattivazione dell'account.  
>   
>  Se è disponibile un provider di posta elettronica integrato, sarà attivato anche l'account di posta elettronica assegnato all'account utente.  
  
###  <a name="BKMK_Manage6"></a>Disattivare un account utente  
 Quando si disattiva un account utente, l'accesso dell'account al server sarà sospeso temporaneamente. L'utente assegnato non potrà quindi usare l'account per accedere alle risorse di rete, ad esempio le cartelle condivise o il sito di Accesso Web remoto, fino all'attivazione dell'account.  
  
 Se all'account utente è assegnato un account online Microsoft, sarà disattivato anche l'account online. L'utente non può usare le risorse in Office 365 e altri Servizi online sottoscritti, ma i dati dell'utente, inclusa la posta elettronica, vengono conservati in Microsoft Online Services.  
  
> [!NOTE]
>  È possibile disattivare solo un account utente attualmente attivo.  
  
##### <a name="to-deactivate-a-user-account"></a>Per disattivare un account utente  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nella visualizzazione elenco selezionare l'account utente da disattivare.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Disattiva account utente**.  
  
5.  Nella finestra di conferma fare clic su **Sì** per confermare l'azione.  
  
> [!NOTE]
>  Dopo la disattivazione di un account utente, lo stato dell'account sarà **Inattivo**.  
>   
>  Se è disponibile un provider di posta elettronica integrato, sarà disattivato anche l'account di posta elettronica assegnato all'account utente.  
  
###  <a name="BKMK_Manage7"></a>Informazioni sugli account utente  
 Un account utente fornisce informazioni importanti a Windows Server Essentials, permettendo ai singoli utenti di accedere a informazioni archiviate sul server e di creare e gestire i propri file e le proprie impostazioni. Gli utenti possono accedere a qualsiasi computer in rete, se hanno un account utente di Windows Server Essentials e le autorizzazioni necessarie per accedere a un computer specifico. Gli utenti accedono agli account utente con il proprio nome utente e la password.  
  
 Sono disponibili due tipi principali di account utente. Ogni tipo concede agli utenti un livello di controllo diverso per il computer:  
  
-   Gli account**standard** sono ideali per un uso quotidiano del computer. L'account standard semplifica la protezione della rete, impedendo agli utenti di apportare modifiche che possono influire su altri utenti, ad esempio l'eliminazione di file o la modifica delle impostazioni di rete.  
  
-   Gli account **Administrator** offrono il livello massimo di controllo su una rete di computer. È consigliabile assegnare il tipo di account Administrator solo se necessario.  
  
###  <a name="BKMK_Manage8"></a>Gestire gli account utente tramite il dashboard  
 Windows Server Essentials permette di eseguire attività amministrative comuni tramite il dashboard di Windows Server Essentials. Per impostazione predefinita, la pagina **utenti** del dashboard include due schede, ovvero **utenti** e **gruppi di utenti**.  
  
> [!NOTE]
> - Se si integra il server che esegue Windows Server Essentials con Office 365, viene aggiunta anche una nuova scheda denominata **gruppi di distribuzione** all'interno della pagina **utenti** del dashboard.  
>   -   In Windows Server Essentials la pagina **utenti** del dashboard include solo una singola scheda, ovvero **utenti**.  
  
 Nella scheda **Utenti** sono disponibili gli elementi seguenti:  
  
- Un elenco di account utente, che include:  
  
  -   Il nome dell'utente.  
  
  -   Il nome di accesso per l'account utente.  
  
  -   Indicazioni sulla disponibilità o meno delle autorizzazioni di tipo Accesso remoto via Internet per l'account utente. L'autorizzazione di tipo Accesso remoto via Internet per un account utente può essere impostata su **Consentito** o **Non consentito**.  
  
  -   Indicazioni sulla gestione o meno della Cronologia file per questo account utente da parte del server che esegue Windows Server Essentials. Lo stato di Cronologia file per un account utente può essere **Gestito** o **Non gestito**.  
  
  -   Il livello di accesso assegnato all'account utente. È possibile assegnare a un account utente il tipo di accesso **Utente standard** o **Administrator**.  
  
  -   Lo stato dell'account utente. Un account utente può essere **Attivo**, **Inattivo** o **Incompleto**.  
  
  -   In Windows Server Essentials, se il server è integrato con Office 365 o Windows Intune, viene visualizzato l'account online Microsoft.  
  
  -   In Windows Server Essentials, se il server è integrato con Microsoft Office 365, viene visualizzato lo stato dell'account Office 365 (noto in Windows Server Essentials come account online Microsoft) per l'account utente.  
  
- Un riquadro dei dettagli che include informazioni aggiuntive sull'account utente selezionato.  
  
- Un riquadro attività che include:  
  
  -   Un insieme di attività amministrative dell'account utente, ad esempio la visualizzazione e la rimozione di account utente e la modifica delle password.  
  
  -   Attività che permettono di impostare o modificare a livello globale le impostazioni per tutti gli account utente nella rete.  
  
  Nella tabella seguente vengono descritte le varie attività dell'account utente disponibili nella scheda **utenti** . Alcune attività sono specifiche dell'account utente e sono visibili solo quando si seleziona un account utente nell'elenco.  
  
> [!NOTE]
>  Se si integra Office 365 con Windows Server Essentials, verranno rese disponibili attività aggiuntive. Per altre informazioni, vedere [gestire gli account online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
### <a name="user-account-tasks-in-the-dashboard"></a>Attività dell'account utente nel dashboard  
  
|Nome dell'attività|Descrizione|  
|---------------|-----------------|  
|Visualizza proprietà account|Permette di visualizzare e modificare le proprietà dell'account utente selezionato e di specificare le autorizzazioni di accesso alla cartella per l'account.|  
|Disattiva account utente|Un account utente disattivato non potrà accedere alla rete o alle risorse di rete, quali le cartelle condivise o le stampanti.|  
|Attiva account utente|Un account utente attivato può accedere alla rete e alle risorse di rete, in base a quanto definito dalle autorizzazioni dell'account.|  
|Rimuovi account utente|Permette di rimuovere l'account utente selezionato.|  
|Modifica password dell'account utente|Permette di reimpostare la password di rete per l'account utente selezionato.|  
|Aggiungere un account utente|Avvia la procedura guidata Aggiungi account utente, che permette di creare un nuovo account utente singolo con accesso utente standard o accesso Administrator.|  
|Assegna un account di Microsoft online|Aggiunge un account online Microsoft all'account utente di rete locale selezionato.<br /><br /> Questa attività è visualizzata quando il server è integrato con i servizi online Microsoft, ad esempio con Office 365.|  
|Aggiungi account di Microsoft online|Aggiunge account online Microsoft e li associa agli account utente di rete locali.<br /><br /> Questa attività è visualizzata quando il server è integrato con i servizi online Microsoft, ad esempio con Office 365.|  
|Imposta criteri password|Permette di modificare i valori dei criteri per le password per la rete.|  
|Importa account Microsoft online|Esegue un'importazione in blocco degli account da Microsoft Online Services alla rete locale.<br /><br /> Questa attività è visualizzata quando il server è integrato con i servizi online Microsoft, ad esempio con Office 365.|  
|Aggiorna|Aggiorna la scheda Utenti.<br /><br /> Questa attività è applicabile a Windows Server Essentials.|  
|Modifica Impostazioni di Cronologia file|Permette di cambiare le impostazioni di Cronologia file, ad esempio la frequenza o la durata del backup.<br /><br /> Questa attività è applicabile a Windows Server Essentials.|  
|Esporta tutte le connessioni remote|Crea un file in formato CSV che include tutte le connessioni remote al server relative agli ultimi 30 giorni.|  
  
##  <a name="BKMK_ManageAccess"></a>Gestione delle password e dell'accesso  
 Gli argomenti seguenti includono informazioni su come usare il Dashboard di Windows Server Essentials per gestire le password degli account utente e l'accesso utente alle cartelle condivise sul server:  
  
-   [Modificare o reimpostare la password per un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [Cosa si deve sapere sui criteri password](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [Modificare i criteri per le password](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [Livello di accesso alle cartelle condivise](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [Mantenere e gestire l'accesso ai file per gli account utente rimossi](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [Sincronizzare la password della modalità ripristino servizi directory con la password dell'amministratore di rete](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [Concedere agli account utente l'autorizzazione desktop remoto](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [Consentire agli utenti di accedere alle risorse sul server](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [Modificare le autorizzazioni di accesso remoto per un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [Modificare le autorizzazioni di rete privata virtuale per un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [Modificare l'accesso alle cartelle condivise interne per un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [Consenti agli account utente di stabilire una sessione di desktop remoto nel computer](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a>Modificare o reimpostare la password per un account utente  
 Per modificare o reimpostare la password per un account utente. eseguire la procedura seguente.  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>Per reimpostare la password per un account utente  
  
1. Aprire il dashboard di Windows Server Essentials.  
  
2. Sulla barra di spostamento fare clic su **Utenti**.  
  
3. Nell'elenco di account utente selezionare l'account utente da reimpostare.  
  
4. Nel riquadro **attività < account utente\>** , fare clic su **modifica password dell'account utente**. Sarà visualizzata la procedura guidata Cambia password account utente.  
  
5. Digitare una nuova password per l'account utente, quindi digitarla di nuovo per confermarla.  
  
6. Fare clic su **Cambia password**.  
  
7. Fornire la nuova password all'utente.  
  
   > [!IMPORTANT]
   > - Se i criteri per le password per l'account sono stati impostati su **Le password non hanno scadenza**, è possibile che non sia permessa la modifica della password.  
   >   -   I caratteri non ASCII non sono supportati in Azure AD. Pertanto, se il server è integrato con Azure AD, non usare caratteri non ASCII nella password.  
   >   -   Se all'utente viene assegnato un account online Microsoft (noto in Windows Server Essentials come account Office 365), la password viene sincronizzata con la password dell'account online. L'utente userà la nuova password per accedere al server o a Office 365. Per altre informazioni, vedere [gestire gli account online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
###  <a name="BKMK_Access3"></a>Cosa si deve sapere sui criteri password  
 I criteri per le password sono un insieme di regole che definiscono il modo in cui gli utenti possono creare e usare le password. I criteri aiutano a evitare l'accesso non autorizzato ai dati degli utenti e ad altre informazioni archiviate sul server. I criteri per le password sono applicati a tutti gli account utente che accedono alla rete.  
  
 I criteri per le password di Windows Server Essentials sono costituiti da tre elementi principali, come indicato di seguito:  
  
- **Lunghezza della password**.  A una maggiore lunghezza della password corrisponde una maggiore sicurezza. Le password vuote non sono sicure.  
  
- **Complessità della password**.  Le password complesse includono una combinazione di lettere maiuscole e minuscole (a-z, A-Z), numeri di base (0-9) e simboli non alfabetici (quali !,@,#,_,-). Le password complesse sono meno suscettibili all'accesso non autorizzato. Le password che contengono i nomi, i compleanni o altre informazioni personali dell'utente non offrono una sicurezza adeguata.  
  
- **Validità della password**.  In Windows Server Essentials gli utenti devono modificare la password almeno una volta ogni 180 giorni. Se si vuole, è possibile scegliere di usare password senza scadenza.  
  
  Per semplificare l'implementazione dei criteri per le password nella rete di computer, in Windows Server Essentials è disponibile un semplice strumento che permette di impostare o modificare i criteri per le password usando uno dei quattro profili predefiniti seguenti per i criteri:  
  
- **Bassa**.  Gli utenti possono specificare qualsiasi password non vuota.  
  
- **Media**.  Queste password devono contenere almeno cinque caratteri. Non sono necessarie password complesse.  
  
- **Mediamente complessa**.  Queste password devono contenere almeno cinque caratteri e devono includere lettere, numeri e simboli.  
  
- **Alta**.  Queste password devono contenere almeno sette caratteri e devono includere lettere, numeri e simboli. Queste password sono più sicure ma potrebbero essere difficili da ricordare.  
  
  > [!NOTE]
  >  Le password non possono includere il nome o l'indirizzo di posta elettronica dell'utente.  
  > 
  >  Se si esegue l'integrazione con Office 365, saranno applicati i criteri per le password di tipo **Alta** e i criteri saranno aggiornati in modo da includere i requisiti seguenti:  
  > 
  > - Le password devono contenere 8 16 caratteri.  
  >   -   Le password non possono includere spazi o l'alias di posta elettronica di Office 365.  
  
  Per impostazione predefinita, l'installazione del server imposta i criteri predefiniti per le password sull'opzione **Alta**.  
  
###  <a name="BKMK_Access4"></a>Modificare i criteri per le password  
 Eseguire la procedura seguente per impostare o modificare i criteri per le password specificando uno dei quattro profili predefiniti per i criteri.  
  
##### <a name="to-change-the-password-policy"></a>Per modificare i criteri per le password  
  
1.  Aprire il Dashboard di Windows Server Essentials, quindi fare clic su **Utenti**.  
  
2.  Nel riquadro **Attività utente** fare clic su **Imposta criteri password**.  
  
3.  Nella schermata **Modifica criteri password** impostare il livello di complessità della password spostando il dispositivo di scorrimento.  
  
     Microsoft consiglia di impostare la complessità della password su **Alta**.  
  
    > [!NOTE]
    >  È anche possibile selezionare l'opzione **Le password non hanno scadenza**. Questa impostazione è meno sicura, quindi non è consigliata.  
  
4.  Fare clic su **Modifica criterio**.  
  
###  <a name="BKMK_Access5"></a>Livello di accesso alle cartelle condivise  
 È consigliabile assegnare le autorizzazioni più restrittive disponibili, che tuttavia permettono comunque agli utenti di eseguire le attività necessarie.  
  
 Sono disponibili tre impostazioni per l'accesso alle cartelle condivise sul server:  
  
-   **Lettura/Scrittura**.  Selezionare questa impostazione se si vuole permettere all'account utente di creare, modificare ed eliminare i file disponibili nella cartella condivisa.  
  
-   **Sola lettura**.  Selezionare questa impostazione se si vuole permettere all'account utente solo di leggere i file disponibili nella cartella condivisa. Gli account utente con accesso di sola lettura non possono creare, modificare o eliminare eventuali file disponibili nella cartella condivisa.  
  
-   **Nessun accesso**.  Selezionare questa impostazione se non si vuole che l'account utente acceda ai file disponibili nella cartella condivisa.  
  
###  <a name="BKMK_Access6"></a>Mantenere e gestire l'accesso ai file per gli account utente rimossi  
 L'amministratore di rete può rimuovere un account utente e scegliere di tenere i file dell'utente per un uso futuro. In questo scenario l'account utente rimosso non potrà essere più usato per accedere alla rete. I file relativi a questo utente, tuttavia, saranno salvati in una cartella condivisa, che può essere condivisa con un altro utente.  
  
> [!IMPORTANT]
>  Occorre notare che se si rimuove un account utente a cui è assegnato un account online Microsoft, sarà rimosso anche l'account online e i dati dell'utente, inclusa la posta elettronica, saranno soggetti ai criteri di conservazione applicati in Microsoft Online Services. Per conservare i dati utente per l'account online, disattivare l'account utente invece di rimuoverlo. Per altre informazioni, vedere [gestire gli account online per gli utenti](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-users-files"></a>Per rimuovere un account utente ma conservare l'accesso ai file dell'utente  
  
1. Aprire il dashboard di Windows Server Essentials.  
  
2. Sulla barra di spostamento fare clic su **Utenti**.  
  
3. Nell'elenco di account utente selezionare l'account utente da rimuovere.  
  
4. Nel riquadro **attività < account utente\>** , fare clic su **Rimuovi account utente**. Sarà visualizzata la procedura guidata Eliminazione account utente.  
  
5. Nella pagina **Conservare i file?** verificare che la casella di controllo **Elimina i file inclusi nei backup di cronologia file e reindirizza la cartella per questo account utente** sia deselezionata, quindi fare clic su **Avanti**.  
  
    Sarà visualizzata una pagina di conferma, che avvisa l'account sarà eliminato ma i file saranno conservati.  
  
6. Fare clic su **Elimina account** per rimuovere l'account utente.  
  
   Dopo la rimozione dell'account utente, l'amministratore può concedere a un altro account utente l'accesso alla cartella condivisa.  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>Per concedere a un account utente l'autorizzazione per accedere a una cartella condivisa  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Archiviazione** e quindi sulla scheda **Cartelle server**.  
  
3.  Nell'elenco di cartelle selezionare la cartella **Utenti**.  
  
4.  Nel riquadro **Attività utente** fare clic su **Apri la cartella**. Esplora risorse sarà aperto e saranno visualizzati i contenuti della cartella **Utenti**.  
  
5.  Fare clic con il pulsante destro del mouse sulla cartella per l'account utente da condividere, quindi scegliere **Proprietà**.  
  
6.  In **< account utente\> proprietà**, fare clic sulla scheda **condivisione** e quindi fare clic su **Condividi**.  
  
7.  Nella finestra **Condivisione file** digitare o selezionare il nome dell'account utente con cui si vuole condividere la cartella, quindi fare clic su **Aggiungi**.  
  
8.  Scegliere il **Livello di autorizzazione** da assegnare all'account utente, quindi fare clic su **Condividi**.  
  
###  <a name="BKMK_Access7"></a>Sincronizzare la password della modalità ripristino servizi directory con la password dell'amministratore di rete  
 La modalità ripristino servizi directory (DSRM, Directory Services Restore Mode) è una modalità di avvio speciale per il ripristino o il recupero di Active Directory. Il sistema operativo usa la modalità ripristino servizi directory per accedere al computer in caso di errore o di necessità di ripristino di Active Directory. Se la password dell'amministratore di rete è diversa da quella per la modalità ripristino servizi directory, non sarà possibile caricare questa modalità.  
  
 Durante un'installazione pulita o la prima installazione di Windows Server Essentials, il programma imposta la password per la modalità ripristino servizi directory sulla password dell'account dell'amministratore di rete specificata durante la configurazione o nel file di risposta della migrazione. Quando si modifica la password dell'amministratore di rete, come consigliato in genere ogni 60 giorni per una maggiore sicurezza del server, la modifica della password non sarà inoltrata alla modalità ripristino servizi directory. Le due password non saranno quindi uguali. In tal caso, è possibile utilizzare le soluzioni seguenti per sincronizzare manualmente o automaticamente la password dell'amministratore di rete con la password della modalità ripristino servizi directory.  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Per sincronizzare manualmente la password della modalità ripristino servizi directory con un account di amministratore di rete  
  
1. Al prompt dei comandi eseguire `ntdsutil.exe` per aprire lo strumento ntdsutil.  
  
2. Per reimpostare la password della modalità ripristino servizi directory, digitare **set dsrm password**.  
  
3. Per sincronizzare la password della modalità ripristino servizi directory in un controller di dominio con l'account dell'amministratore di rete corrente, digitare:  
  
    **sincronizzare dall'account di dominio** *< current_network_administrator_account >* , quindi premere INVIO.  
  
   Poiché la password per l'account dell'amministratore di rete sarà modificata periodicamente, per assicurare che la password della modalità ripristino servizi directory corrisponda sempre alla password attuale dell'amministratore di rete, è consigliabile creare un'attività pianificata per sincronizzare automaticamente ogni giorno la password della modalità ripristino servizi directory con la password dell'amministratore di rete.  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Per sincronizzare automaticamente la password della modalità ripristino servizi directory con un account di amministratore di rete  
  
1.  Nel server aprire **Strumenti di amministrazione**, quindi fare doppio clic su **Utilità di pianificazione**.  
  
2.  Nel riquadro **Azioni** dell'Utilità di pianificazione fare clic su **Crea attività**.  
  
3.  Nella casella di testo **Nome** digitare un nome per l'attività, ad esempio **Password DSRM con SincrAut**, quindi selezionare l'opzione **Esegui con i privilegi più elevati**.  
  
4.  Definire quando deve essere eseguita l'attività:  
  
    1.  Nella finestra di dialogo **Crea attività** fare clic sulla scheda **Trigger** e quindi su **Nuovo**.  
  
    2.  Nella finestra di dialogo **Nuovo trigger** selezionare l'opzione relativa alla ricorrenza, specificare l'intervallo di ricorrenza e scegliere un'ora di inizio.  
  
        > [!NOTE]
        >  È consigliabile impostare l'attività per l'esecuzione giornaliera al di fuori dell'orario di lavoro.  
  
    3.  Fare clic su **OK** per salvare le modifiche e tornare alla finestra di dialogo **Crea attività**.  
  
5.  Definire le azioni dell'attività:  
  
    1.  Fare clic sulla scheda **Azioni** e quindi su **Avanti**. Sarà visualizzata la finestra di dialogo **Nuova azione**.  
  
    2.  Nell'elenco **Azione** fare clic su **Avvia programma**, quindi passare a **C:\WINDOWS\SYSTEM32\ntdsutil.exe**.  
  
    3.  Nella casella di testo **Aggiungi argomenti**(facoltativo) digitare quanto segue (è necessario includere le virgolette): **impostare la sincronizzazione password per la modalità ripristino servizi directory dall'account di dominio SBS_network_administrator_account q q** dove *SBS_network_administrator_account* è il nome dell'account dell'amministratore di rete corrente.  
  
6.  Fare due volte clic su **OK** per salvare l'attività e chiudere la finestra di dialogo **Crea attività**. La nuova attività sarà visualizzata nella sezione **Attività attive** dell'**Utilità di pianificazione**.  
  
###  <a name="BKMK_Access8"></a>Concedere agli account utente l'autorizzazione desktop remoto  
 Nell'installazione predefinita di Windows Server Essentials gli utenti di rete non sono autorizzati a stabilire una connessione remota ai computer o alle risorse in rete.  
  
 Per permettere agli utenti di rete di stabilire una connessione remota alle risorse di rete, occorre prima di tutto configurare l'Accesso remoto via Internet. Dopo la configurazione dell'Accesso remoto via Internet, gli utenti potranno accedere a file, applicazioni e computer disponibili nella rete aziendale da un dispositivo che si trova ovunque sia disponibile una connessione Internet.  
  
 La procedura guidata Configura Accesso remoto via Internet permette di abilitare due metodi per l'accesso remoto:  
  
- Rete privata virtuale (VPN, Virtual Private Network)  
  
- Accesso Web remoto  
  
  Quando si esegue la procedura guidata, è anche possibile scegliere di abilitare l'Accesso remoto via Internet per tutti gli account utente attuali e appena aggiunti.  
  
  Per configurare l'Accesso remoto via Internet, aprire la pagina **Home** del dashboard, fare clic su **CONFIGURA** e quindi su **Configura Accesso remoto via Internet**.  
  
  Per altre informazioni sull'accesso remoto via Internet, vedere [gestire l'accesso remoto via Internet](Manage-Anywhere-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_Access9"></a>Consentire agli utenti di accedere alle risorse sul server  
  Questa sezione si applica a un server che esegue Windows Server Essentials o Windows Server Essentials o a un server che esegue Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato.  
  
 Per permettere agli utenti di usare l'accesso remoto e/o di disporre di account utente individuali, al termine della connessione di un computer al server sarà possibile creare nuovi account utente di rete per gli utenti del computer in rete sul server tramite il dashboard. Per altre informazioni sulla creazione di un account utente, vedere [Aggiungere un account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1). Dopo la creazione degli account utente, sarà necessario fornire le informazioni sul nome dell'utente di rete e sulla password agli utenti del computer client, in modo che possano accedere alle risorse sul server tramite la finestra di avvio.  
  
 Per ogni account utente creato è possibile impostare l'accesso alle risorse seguenti tramite le proprietà dell'account utente:  
  
-   **Cartelle condivise**.  Per impostazione predefinita, gli amministratori di rete hanno autorizzazioni di **Lettura/Scrittura** per tutte le cartelle condivise e gli account standard hanno autorizzazioni di **Sola lettura** per la cartella Società. Se i flussi multimediali sono abilitati, sarà possibile assegnare le autorizzazioni di accesso alle cartelle per singoli account utente standard per le cartelle condivise seguenti: **Musica**, **Immagini**, **Registrazioni**e **Video**. È possibile impostare le autorizzazioni per permettere agli account utente di accedere alle cartelle condivise nella scheda **Cartelle condivise** delle proprietà dell'account utente.  
  
-   **Accesso remoto via Internet**.  Per impostazione predefinita, gli amministratori di rete possono usare la rete privata virtuale o Accesso Web remoto per accedere alle risorse del server. Per gli account utente standard è necessario impostare le autorizzazioni per l'account utente nella scheda **Accesso remoto via Internet**.  
  
-   **Accesso computer**.  Per impostazione predefinita, gli amministratori di rete possono accedere a tutti i computer della rete. Per gli account utente standard, tuttavia, è possibile impostare autorizzazioni individuali per gli account utente per l'accesso ai computer della rete tramite la scheda **Accesso computer** delle proprietà dell'account utente.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>Per modificare le proprietà dell'account utente in Windows Server Essentials 2012 R2  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **UTENTI**.  
  
3.  Nell'elenco di account utente selezionare quello da modificare.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Visualizza proprietà account**.  
  
5.  Nell' **< account utente\> proprietà**eseguire le operazioni seguenti:  
  
    1.  Nella scheda **Cartelle condivise** impostare le autorizzazioni di cartella appropriate per ogni cartella condivisa, in base alla necessità.  
  
    2.  Nella scheda **Accesso remoto via Internet**:  
  
        1.  Per permettere a un utente di connettersi al server tramite la VPN, selezionare la casella di controllo **Consenti rete privata virtuale (VPN)** .  
  
        2.  Per permettere a un utente di connettersi al server tramite Accesso Web remoto, selezionare la casella di controllo **Consenti Accesso Web remoto e accedi ai servizi Web**.  
  
    3.  Nella scheda **Accesso computer** selezionare i computer di rete a cui l'utente potrà accedere.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>Per modificare le proprietà dell'account utente in Windows Server Essentials 2012  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **UTENTI**.  
  
3.  Nell'elenco di account utente selezionare quello da modificare.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Proprietà**.  
  
5.  Nell' **< account utente\> proprietà**eseguire le operazioni seguenti:  
  
    1.  Nella scheda **Generale** selezionare**L'utente può visualizzare gli avvisi di integrità** se l'account utente deve accedere ai report sull'integrità della rete.  
  
    2.  Nella scheda **Cartelle condivise** impostare le autorizzazioni di cartella appropriate per ogni cartella condivisa, in base alla necessità.  
  
    3.  Nella scheda **Accesso remoto via Internet**:  
  
        1.  Per permettere a un utente di connettersi al server tramite la VPN, selezionare la casella di controllo **Consenti rete privata virtuale (VPN)** .  
  
        2.  Per permettere a un utente di connettersi al server tramite Accesso Web remoto, selezionare la casella di controllo **Consenti Accesso Web remoto e accedi ai servizi Web**.  
  
    4.  Nella scheda **Accesso computer** selezionare i computer di rete a cui l'utente potrà accedere.  
  
###  <a name="BKMK_Access10"></a>Modificare le autorizzazioni di accesso remoto per un account utente  
 Un utente può accedere alle risorse situate sul server da una posizione remota usando una rete privata virtuale (VPN), Accesso Web remoto o altre applicazioni dei servizi Web. Per impostazione predefinita, le autorizzazioni di accesso remoto sono attivate per gli utenti di rete quando si configura l'Accesso remoto via Internet in Windows Server Essentials tramite il dashboard.  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>Per modificare le autorizzazioni di accesso remoto per un account utente  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nell'elenco di account utente selezionare quello da modificare.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Visualizza proprietà account**. Sarà visualizzata la pagina **Proprietà** per l'account utente.  
  
5.  Nella scheda **Accesso remoto via Internet** eseguire le operazioni seguenti:  
  
    -   Selezionare la casella di controllo **Consenti rete privata virtuale (VPN)** per permettere a un utente di connettersi al server tramite la VPN.  
  
    -   Selezionare la casella di controllo **Consenti Accesso Web remoto e accedi ai servizi Web** per permettere a un utente di connettersi al server tramite Accesso Web remoto.  
  
6.  Fare clic su **Applica** e quindi su **OK**.  
  
###  <a name="BKMK_Access11"></a>Modificare le autorizzazioni di rete privata virtuale per un account utente  
 Si può usare una VPN per connettersi a Windows Server Essentials e accedere a tutte le risorse archiviate sul server. Ciò è particolarmente utile se si usa un computer client configurato con account di rete che possono essere usati per connettersi a un server Windows Server Essentials ospitato tramite una connessione VPN. Tutti i nuovi account utente creati nel server Windows Server Essentials ospitato devono usare la VPN per accedere al computer client per la prima volta.  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>Per modificare le autorizzazioni della VPN per gli utenti di rete  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **UTENTI**.  
  
3.  Nell'elenco di account utente selezionare l'account utente a cui si vogliono concedere le autorizzazioni per accedere al desktop in remoto.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Proprietà**.  
  
5.  Nell' **< account utente\> proprietà**fare clic sulla scheda **accesso remoto via Internet** .  
  
6.  Per permettere a un utente di connettersi al server tramite la VPN, nella scheda **Accesso remoto via Internet** selezionare la casella di controllo **Consenti rete privata virtuale (VPN)** .  
  
7.  Fare clic su **Applica** e quindi su **OK**.  
  
###  <a name="BKMK_Access12"></a>Modificare l'accesso alle cartelle condivise interne per un account utente  
 È possibile gestire l'accesso a qualsiasi cartella condivisa sul server tramite le attività disponibili nella scheda **Cartelle server** del dashboard. Per impostazione predefinita, le cartelle del server seguenti vengono create quando si installa Windows Server Essentials:  
  
-   **Backup computer client**.  Permette di archiviare i backup dei computer client creati da Windows Server Backup. Questa cartella del server non è condivisa.  
  
-   **Società**.  Permette l'archiviazione e l'accesso ai documenti correlati all'organizzazione da parte degli utenti di rete.  
  
-   **Backup Cronologia file**.  Per impostazione predefinita, Windows Server Essentials archivia i backup dei file creati usando Cronologia file. Questa cartella del server non è condivisa.  
  
-   **Reindirizzamento cartelle**.  Permette l'archiviazione e l'accesso a cartelle configurate per il reindirizzamento da parte degli utenti di rete. Questa cartella del server non è condivisa.  
  
-   **Musica**.  Permette l'archiviazione e l'accesso a file musicali da parte degli utenti di rete. Questa cartella viene creata quando si attiva la condivisione dei file multimediali.  
  
-   **Immagini**.  Permette l'archiviazione e l'accesso alle immagini da parte degli utenti di rete. Questa cartella viene creata quando si attiva la condivisione dei file multimediali.  
  
-   **Registrazioni**.  Permette l'archiviazione e l'accesso ai programmi televisivi registrati da parte degli utenti di rete. Questa cartella viene creata quando si attiva la condivisione dei file multimediali.  
  
-   **Video**.  Permette l'archiviazione e l'accesso ai video da parte degli utenti di rete. Questa cartella viene creata quando si attiva la condivisione dei file multimediali.  
  
-   **Utenti**.  Permette l'archiviazione e l'accesso ai file da parte degli utenti di rete. Una cartella specifica per l'utente è generata automaticamente nella cartella del server **Utenti** per ogni account utente di rete creato.  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>Per modificare l'accesso a una cartella condivisa per un account utente  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Fare clic su **ARCHIVIAZIONE**, quindi su **Cartelle server**.  
  
3.  Passare alla cartella del server per cui si vogliono modificare le autorizzazioni e selezionarla.  
  
4.  Nel riquadro attività fare clic su **Visualizza le proprietà della cartella**.  
  
5.  In **< foldername\> proprietà**fare clic su **condivisione**e selezionare il livello di accesso utente appropriato per gli account utente elencati, quindi fare clic su **applica**.  
  
    > [!NOTE]
    >  Non è possibile modificare le autorizzazioni di condivisione per le cartelle del server **Backup Cronologia file**, **Reindirizzamento cartelle** e **Utenti**. Le proprietà di queste cartelle del server non includono quindi la scheda **Condivisione**.  
  
###  <a name="BKMK_Access13"></a>Consenti agli account utente di stabilire una sessione di desktop remoto nel computer  
  Questa sezione si applica a un server che esegue Windows Server Essentials o Windows Server Essentials o a un server che esegue Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato.  
  
 L'amministratore di rete può concedere autorizzazioni agli utenti di rete in modo da permettere loro di accedere ai computer di rete da una posizione remota.  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>Per permettere agli utenti di accedere ai computer di rete da una posizione remota  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **UTENTI**.  
  
3.  Nell'elenco di account utente selezionare l'account utente a cui si vogliono concedere le autorizzazioni per accedere al desktop in remoto.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Proprietà**.  
  
5.  Nell' **< account utente\> proprietà**, fare clic sulla scheda **accesso computer** .  
  
6.  Selezionare i computer a cui si vuole che l'account utente possa accedere in remoto, quindi fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire gli account online per gli utenti](Manage-Online-Accounts-for-Users.md)  
  
-   [Connessione](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
