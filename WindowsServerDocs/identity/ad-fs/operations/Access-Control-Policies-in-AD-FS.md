---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: Criteri di controllo degli accessi in AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c690f81620f97622a2f068b07c36e0a6c59e90d4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190343"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Criteri di controllo di accesso in AD FS per Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Modelli di criteri di controllo di accesso in ADFS  
Active Directory Federation Services supporta ora l'utilizzo di modelli di criteri di controllo di accesso.  Utilizzando i modelli di criteri di controllo di accesso, un amministratore può applicare le impostazioni dei criteri assegnando il modello di criteri a un gruppo di relying party (RPs). Amministratore può inoltre effettuare aggiornamenti al modello di criteri e le modifiche verranno applicate per le relying party automaticamente se non esiste alcuna interazione utente necessita.  
  
## <a name="what-are-access-control-policy-templates"></a>Quali sono i modelli di criteri di controllo di accesso?  
La pipeline di componenti di base ADFS per l'elaborazione di criteri include tre fasi: autenticazione, autorizzazione e attestazione di rilascio. Attualmente, gli amministratori di ADFS sono necessario configurare un criterio per ognuna di queste fasi separatamente.  Anche questo è necessario comprendere le implicazioni di questi criteri e se questi criteri non hanno dipendenze tra. Inoltre, gli amministratori devono capire l'attestazione regola language e autore regole personalizzate per abilitare alcuni criteri semplice o comune (ad esempio, Blocca l'accesso esterno).  
  
I criteri di controllo di accesso dei modelli sono sostituire il vecchio modello in cui gli amministratori devono configurare regole di autorizzazione rilascio utilizzando attestazioni language.  I cmdlet PowerShell precedenti di regole di autorizzazione rilascio vengono mantenuti ma verrà escluse il nuovo modello. Gli amministratori possono scegliere di utilizzare il nuovo modello o il modello precedente.  Il nuovo modello consente agli amministratori di controllare la modalità di concedere l'accesso, inclusa l'applicazione multi-factor authentication.  
  
Modelli di criteri di controllo di accesso utilizzano un modello di autorizzazione.  Questo significa che per impostazione predefinita, nessun utente ha accesso e che l'accesso deve essere concesso in modo esplicito.  Tuttavia, non è semplicemente un all o non consentire.  Gli amministratori possono aggiungere le eccezioni alla regola di autorizzazione.  Ad esempio, un amministratore può desidera concedere l'accesso in base a una rete specifica, questa opzione e specificare l'intervallo di indirizzi IP.  Ma l'amministratore può aggiungere ed eccezione, ad esempio, l'amministratore può aggiungere un'eccezione da una rete specifica e specificare l'intervallo di indirizzi IP.  
  
![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Accesso integrato controllo criteri vs accesso personalizzato controllo criteri modelli  
ADFS include diversi modelli di criteri di controllo di accesso integrato.  Questi destinazione alcuni scenari comuni che hanno lo stesso set di requisiti dei criteri, ad esempio criteri di accesso client per Office 365.  Questi modelli non possono essere modificati.  
  
![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Per garantire una maggiore flessibilità per soddisfare le esigenze aziendali, gli amministratori possono creare i propri accesso modelli di criteri.  Questi nomi possono essere modificati dopo la creazione e modifiche al modello di criteri personalizzati verranno applicate a tutte le richieste che sono controllati da questi modelli di criteri.  Per aggiungere un modello di criteri personalizzata fare clic su Aggiungi dei criteri di controllo di accesso all'interno di gestione AD FS.  
  
Per creare un modello di criteri, un amministratore deve innanzitutto specificare a quali condizioni sarà autorizzata una richiesta di rilascio dei token e/o la delega. Nella tabella seguente vengono visualizzate le opzioni di condizione e azione.   Le condizioni in grassetto possono essere ulteriormente configurate dall'amministratore con valori diversi o nuovi. Amministratore può inoltre specificare eccezioni eventuale. Quando viene soddisfatta una condizione, un'azione di autorizzazione non verrà attivata se si verifica un'eccezione specificata e la richiesta in ingresso soddisfa la condizione specificata nell'eccezione.  
  
|Consentire agli utenti|La differenza| 
| --- | --- | 
 |Da **specifico** rete|Da **specifico** rete<br /><br />Da **specifico** gruppi<br /><br />Dai dispositivi con **specifico** livelli di attendibilità<br /><br />Con **specifico** attestazioni nella richiesta|  
|Da **specifico** gruppi|Da **specifico** rete<br /><br />Da **specifico** gruppi<br /><br />Dai dispositivi con **specifico** livelli di attendibilità<br /><br />Con **specifico** attestazioni nella richiesta|  
|Dai dispositivi con **specifico** livelli di attendibilità|Da **specifico** rete<br /><br />Da **specifico** gruppi<br /><br />Dai dispositivi con **specifico** livelli di attendibilità<br /><br />Con **specifico** attestazioni nella richiesta|  
|Con **specifico** attestazioni nella richiesta|Da **specifico** rete<br /><br />Da **specifico** gruppi<br /><br />Dai dispositivi con **specifico** livelli di attendibilità<br /><br />Con **specifico** attestazioni nella richiesta|  
|E richiedere l'autenticazione a più fattori|Da **specifico** rete<br /><br />Da **specifico** gruppi<br /><br />Dai dispositivi con **specifico** livelli di attendibilità<br /><br />Con **specifico** attestazioni nella richiesta|  
  
Se un amministratore seleziona più condizioni, sono di **AND** relazione. Le azioni si escludono a vicenda e per una regola dei criteri è possibile scegliere solo un'azione. Se l'amministratore seleziona più eccezioni, sono di un **o** relazione. Un paio di regola dei criteri di seguito sono riportati alcuni esempi:  
  
|**Criterio**|**Regole dei criteri**|
| --- | --- |  
|L'accesso Extranet richiede l'autenticazione a più Fattori<br /><br />Sono consentiti tutti gli utenti|**Regola di #1**<br /><br />da **extranet**<br /><br />e con autenticazione a più Fattori<br /><br />Autorizza<br /><br />**Rule#2**<br /><br />da **intranet**<br /><br />Autorizza|  
|Accesso esterno non sono consentiti tranne non FTE<br /><br />È consentito l'accesso Intranet per FTE sul dispositivo all'area di lavoro|**Regola di #1**<br /><br />Da **extranet**<br /><br />e da **non FTE** gruppo<br /><br />Autorizza<br /><br />**Regola #2**<br /><br />da **intranet**<br /><br />e da **all'area di lavoro** dispositivo<br /><br />e da **FTE** gruppo<br /><br />Autorizza|  
|L'accesso Extranet richiede autenticazione a più Fattori, ad eccezione di "amministratore del servizio"<br /><br />Tutti gli utenti sono autorizzati ad accedere|**Regola di #1**<br /><br />da **extranet**<br /><br />e con autenticazione a più Fattori<br /><br />Autorizza<br /><br />Ad eccezione di **gruppo amministrativo di servizio**<br /><br />**Regola #2**<br /><br />sempre<br /><br />Autorizza|  
|dispositivo aggiunto al luogo di lavoro non - l'accesso dalla extranet richiede l'autenticazione a più Fattori<br /><br />Consentire l'infrastruttura di Active Directory per l'accesso intranet ed extranet|**Regola di #1**<br /><br />da **intranet**<br /><br />E da **AD Fabric** gruppo<br /><br />Autorizza<br /><br />**Regola #2**<br /><br />da **extranet**<br /><br />e da **non-all'area di lavoro** dispositivo<br /><br />e da **AD Fabric** gruppo<br /><br />e con autenticazione a più Fattori<br /><br />Autorizza<br /><br />**Regola #3**<br /><br />da **extranet**<br /><br />e da **all'area di lavoro** dispositivo<br /><br />e da **AD Fabric** gruppo<br /><br />Autorizza|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Modello di criteri con parametri senza parametri dei criteri di Visual Studio  
Criteri di controllo di accesso possono essere  
  
Un modello di criteri con parametri è un modello di criteri che dispone di parametri. Un amministratore deve immettere il valore per tali parametri durante l'assegnazione di questo modello RPs.An amministratore non può apportare modifiche al modello di criteri con parametri dopo che è stato creato.  Un esempio di un criterio con parametri è il criterio incorporato, il gruppo specifico licenza.  Ogni volta che questo criterio viene applicato a un'applicazione relying Party, questo parametro deve essere specificata.  
  
![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Un modello di criteri senza parametri è un modello di criteri che non dispone di parametri. Un amministratore può assegnare questo modello RPs senza input necessita e possa apportare modifiche a un modello di criteri senza parametri dopo che è stato creato.  Un esempio di questo è il criterio predefinito, consentire tutti gli utenti e richiedere l'autenticazione a più Fattori.  
  
![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Procedura: creare un criterio di controllo di accesso senza parametri  
Per creare un accesso senza parametri dei criteri di controllo utilizzare la procedura seguente  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Per creare un criterio di controllo di accesso senza parametri  
  
1.  Dalla gestione di ADFS a sinistra selezionare criteri di controllo di accesso e a destra fare clic su Aggiungi criteri di controllo di accesso.  
  
2.  Immettere un nome e una descrizione.  Ad esempio:   Consentire agli utenti con dispositivi autenticati.  
  
3.  In **consentire l'accesso se viene soddisfatta una delle seguenti regole**, fare clic su **Aggiungi**.  
  
4.  In Consenti, inserire un segno di spunta nella casella accanto a **dai dispositivi con livello di attendibilità specifico**  
  
5.  Nella parte inferiore, selezionare il sottolineato **specifico**  
  
6.  Nella finestra che viene visualizzato, selezionare **autenticato** dall'elenco a discesa.  Fare clic su **OK**.  
  
    ![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Fare clic su **OK**. Fare clic su **OK**.  
  
    ![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Procedura: creare un criterio di controllo di accesso con parametri  
Per creare un controllo di accesso con parametri criteri utilizzare la procedura seguente  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Per creare un criterio di controllo di accesso con parametri  
  
1.  Dalla gestione di ADFS a sinistra selezionare criteri di controllo di accesso e a destra fare clic su Aggiungi criteri di controllo di accesso.  
  
2.  Immettere un nome e una descrizione.  Ad esempio:   Consentire agli utenti con una determinata attestazione.  
  
3.  In **consentire l'accesso se viene soddisfatta una delle seguenti regole**, fare clic su **Aggiungi**.  
  
4.  In Consenti, inserire un segno di spunta nella casella accanto a **con le attestazioni specifiche nella richiesta**  
  
5.  Nella parte inferiore, selezionare il sottolineato **specifico**  
  
6.  Nella finestra che viene visualizzato, selezionare **parametro specificato quando viene assegnato il criterio di controllo di accesso**.  Fare clic su **OK**.  
  
    ![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Fare clic su **OK**. Fare clic su **OK**.  
  
    ![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Procedura: creare un criterio di controllo di accesso personalizzata con un'eccezione  
Per creare un controllo di accesso ai criteri con un'eccezione utilizzare la procedura seguente.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Per creare un criterio di controllo di accesso personalizzata con un'eccezione  
  
1.  Dalla gestione di ADFS a sinistra selezionare criteri di controllo di accesso e a destra fare clic su Aggiungi criteri di controllo di accesso.  
  
2.  Immettere un nome e una descrizione.  Ad esempio:   Consentire agli utenti autenticati dispositivi ma non gestito.  
  
3.  In **consentire l'accesso se viene soddisfatta una delle seguenti regole**, fare clic su **Aggiungi**.  
  
4.  In Consenti, inserire un segno di spunta nella casella accanto a **dai dispositivi con livello di attendibilità specifico**  
  
5.  Nella parte inferiore, selezionare il sottolineato **specifico**  
  
6.  Nella finestra che viene visualizzato, selezionare **autenticato** dall'elenco a discesa.  Fare clic su **OK**.  
  
7.  Nella casella except, inserire un segno di spunta nella casella accanto a **dai dispositivi con livello di attendibilità specifico**  
  
8.  Nella parte inferiore in eccetto, selezionare il sottolineato **specifico**  
  
9. Nella finestra che viene visualizzato, selezionare **gestito** dall'elenco a discesa.  Fare clic su **OK**.  
  
10. Fare clic su **OK**. Fare clic su **OK**.  
  
    ![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Procedura: creare un criterio di controllo di accesso personalizzate con più condizioni di licenza  
Per creare un criterio di controllo di accesso con autorizzazione più condizioni di utilizzano la procedura seguente  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Per creare un criterio di controllo di accesso con parametri  
  
1.  Dalla gestione di ADFS a sinistra selezionare criteri di controllo di accesso e a destra fare clic su Aggiungi criteri di controllo di accesso.  
  
2.  Immettere un nome e una descrizione.  Ad esempio:  Consentire agli utenti con una determinata attestazione e dal gruppo specifico.  
  
3.  In **consentire l'accesso se viene soddisfatta una delle seguenti regole**, fare clic su **Aggiungi**.  
  
4.  In Consenti, inserire un segno di spunta nella casella accanto a **da un gruppo specifico** e **con le attestazioni specifiche nella richiesta**  
  
5.  Nella parte inferiore, selezionare il sottolineato **specifico** per la prima condizione, accanto a gruppi  
  
6.  Nella finestra che viene visualizzato, selezionare **parametro specificato quando è assegnato il criterio**.  Fare clic su **OK**.  
  
7.  Nella parte inferiore, selezionare il sottolineato **specifico** per la seconda condizione, accanto a attestazioni  
  
8.  Nella finestra che viene visualizzato, selezionare **parametro specificato quando viene assegnato il criterio di controllo di accesso**.  Fare clic su **OK**.  
  
9. Fare clic su **OK**. Fare clic su **OK**.  
  
![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Come assegnare un criterio di controllo di accesso a una nuova applicazione  
L'assegnazione di un criterio di controllo di accesso a una nuova applicazione è piuttosto semplice e ora è stata integrata nella procedura guidata per l'aggiunta di un'applicazione relying Party.  Da guidata attendibilità il componente è possibile selezionare i criteri di controllo di accesso che si desiderano assegnare.  Questo è un requisito quando si crea un nuovo trust della relying party.  
  
![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Come assegnare un criterio di controllo di accesso a un'applicazione esistente  
L'assegnazione di un criterio di controllo di accesso a un'applicazione esistente semplicemente seleziona l'applicazione dalla Relying Party Trusts e fare clic destro su **Modifica criteri di controllo di accesso**.  
  
![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
Da qui è possibile selezionare i criteri di controllo di accesso e applicarla all'applicazione.  
  
![criteri di controllo di accesso](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Vedere anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 

