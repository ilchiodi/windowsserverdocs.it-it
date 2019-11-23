---
title: Passaggio 2 configurare l'infrastruttura multisito
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b345ce7cdbb0cf9ff91ec99275232da5ba34edb0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367202"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>Passaggio 2 configurare l'infrastruttura multisito

>Si applica a: Windows Server 2012 R2, Windows Server 2012

Per configurare una distribuzione multisito, esistono una serie di passaggi necessari per modificare le impostazioni di infrastruttura di rete tra cui: configurazione di Active Directory siti e i controller di dominio, la configurazione di altri gruppi di sicurezza e la configurazione dei criteri di gruppo (GPO) se non si utilizza automaticamente configurati oggetti Criteri di gruppo.  
  
|Attività|Descrizione|  
|----|--------|  
|2.1. Configurare ulteriori siti di Active Directory|Configurare ulteriori siti di Active Directory per la distribuzione.|  
|2.2. Configurare i controller di dominio aggiuntivo|Configurare ulteriori controller di dominio Active Directory come richiesto.|  
|2.3. Configurare i gruppi di sicurezza|Configurare gruppi di sicurezza per tutti i computer client Windows 7.|  
|2.4. Configurare gli oggetti Criteri di gruppo|Configurare oggetti Criteri di gruppo aggiuntive come richiesto.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigAD"></a>2,1. Configurare ulteriori siti di Active Directory  
Tutti i punti di ingresso possono risiedere in un singolo sito di Active Directory. Pertanto, almeno un sito di Active Directory è richiesto per l'implementazione del server di accesso remoto in una configurazione multisito. Utilizzare questa procedura se è necessario creare il primo sito di Active Directory o se si desidera utilizzare altri siti di Active Directory per la distribuzione multisita. Utilizzare lo snap-in Active Directory Sites and Services di creare nuovi siti nell'organizzazione "network s.  

L'appartenenza di **Enterprise Admins** gruppo nella foresta o **Domain Admins** gruppo dominio radice della foresta, o equivalente, come minimo è necessario per completare questa procedura. Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).  

Per ulteriori informazioni, vedere [aggiunta di un sito alla foresta](https://technet.microsoft.com/library/cc732761.aspx).  

### <a name="to-configure-additional-active-directory-sites"></a>Per configurare ulteriori siti di Active Directory  
  
1.  Nel controller di dominio primario, fare clic su **avviare**, quindi fare clic su **Active Directory Sites and Services**.  
  
2.  Nella console di Active Directory Sites and Services, nell'albero della console, fare doppio clic su **siti**, quindi fare clic su **nuovo sito**.  
  
3.  Nel **nuovo oggetto - sito** della finestra di dialogo di **nome** immettere un nome per il nuovo sito.  
  
4.  In **Nome collegamento**, fare clic su un oggetto collegamento di sito e quindi fare clic su **OK** due volte.  
  
5.  Nell'albero della console, espandere **siti**, fare doppio clic su **subnet**, quindi fare clic su **nuova Subnet**.  
  
6.  Nel **nuovo oggetto - Subnet** nella finestra di dialogo **prefisso**, digitare il prefisso di subnet IPv4 o IPv6, il **Selezionare un oggetto sito per il prefisso** elenco, fare clic sul sito per associare questa subnet e quindi fare clic su **OK**.  
  
7.  Ripetere i passaggi 5 e 6 fino a creare tutte le subnet necessarie nella distribuzione.  
  
8.  Chiudere siti e servizi Active Directory.  
  
![](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Per installare la funzionalità di Windows "Modulo Active Directory per Windows PowerShell":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
oppure aggiungere "Active Directory PowerShell Snap-In" tramite OptionalFeatures.  
  
Se in esecuzione i seguenti cmdlet di Windows Server 2008 R2 o Windows 7", è necessario importare il modulo di PowerShell per Active Directory:  
  
```  
Import-Module ActiveDirectory  
```  
  
Per configurare un sito Active Directory denominato "Secondo sito" utilizzando DEFAULTIPSITELINK incorporato:  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
Per configurare le subnet IPv4 e IPv6 per il sito secondo:  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="BKMK_AddDC"></a>2,2. Configurare i controller di dominio aggiuntivo  
Per configurare una distribuzione multisito in un singolo dominio, è consigliabile avere almeno un controller di dominio scrivibile per ogni sito nella distribuzione.  
  
Per eseguire questa procedura, come minimo che è necessario essere un membro del gruppo Domain Admins nel dominio in cui viene installato il controller di dominio.  
  
Per ulteriori informazioni, vedere [l'installazione di un Controller di dominio aggiuntivo](https://technet.microsoft.com/library/cc733027.aspx).
  
### <a name="to-configure-additional-domain-controllers"></a>Per configurare i controller di dominio aggiuntivo  
  
1.  Nel server che fungerà da un controller di dominio in **Server Manager**, via il **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** tre volte per visualizzare la schermata di selezione ruoli server  
  
3.  Nel **Selezione ruoli Server** selezionare **servizi di dominio Active Directory**. Fare clic su **Aggiungi funzionalità** quando richiesto e quindi fare clic su **Avanti** tre volte.  
  
4.  Nella pagina **Conferma** fare clic su **Installa**.  
  
5.  Al termine dell'installazione, fare clic su **promuovere il server a un controller di dominio**.  
  
6.  In Active Directory Domain Services configurazione guidata, nel **configurazione della distribuzione** pagina, fare clic su **aggiungere un controller di dominio a un dominio esistente**.  
  
7.  In **dominio**, immettere il dominio nome, ad esempio, corp.contoso.com.  
  
8.  In **fornire le credenziali per eseguire questa operazione**, fare clic su **Modifica**. Nel **la protezione di Windows** finestra di dialogo immettere il nome utente e la password per un account che è possibile installare il controller di dominio aggiuntivo. Per installare un controller di dominio aggiuntivo è necessario appartenere al gruppo Enterprise Admins o al gruppo Domain Admins. Dopo aver immesso le credenziali, fare clic su **Avanti**.  
  
9. Nel **Opzioni Controller di dominio** pagina, effettuare le seguenti operazioni:  
  
    1.  Effettuare le selezioni seguenti:  
  
        -   **Server Domain Name System (DNS)** "questa opzione è selezionata per impostazione predefinita, in modo che il controller di dominio possa operare come un server Domain Name System (DNS). Se non si desidera utilizzare il controller di dominio come server DNS, deselezionare questa opzione.  
  
            Se il ruolo server DNS non è installato sull'emulatore del Controller di dominio primario (PDC) nel dominio radice della foresta, l'opzione per installare il server DNS in un controller di dominio non è disponibile. Come soluzione alternativa in questo caso, è possibile installare il ruolo server DNS prima o dopo l'installazione di Active Directory.  
  
            > [!NOTE]  
            > Se si seleziona l'opzione per installare il server DNS, si potrebbe ricevere un messaggio che indica che non è stato possibile creare una delega DNS per il server DNS e che è necessario creare manualmente una delega DNS per il server DNS per garantire la risoluzione dei nomi affidabile. Se si installa un controller di dominio nel dominio radice della foresta o un dominio radice della struttura ad albero, non è necessario creare la delega DNS. In questo caso, fare clic su **Sì** e ignorare il messaggio.  
  
        -   **Catalogo globale (GC)** "questa opzione è selezionata per impostazione predefinita. Consente di aggiungere il catalogo globale e le partizioni di directory di sola lettura al controller di dominio, nonché di abilitare la funzionalità di ricerca nel catalogo globale.  
  
        -   **Controller di dominio di sola lettura (RODC)** "questa opzione non è selezionata per impostazione predefinita. Rende il controller di dominio aggiuntivo in sola lettura. ovvero, in questo modo il controller di dominio un RODC.  
  
    2.  In **nome sito**, selezionare un sito dall'elenco.  
  
    3.  In **digitare la password modalità ripristino servizi Directory (DSRM)** , in **Password** e **Conferma password**, digitare due volte una password complessa e quindi fare clic su **Avanti**. Questa password deve essere utilizzata per avviare Active Directory in modalità ripristino servizi Directory per le attività che devono essere eseguite non in linea.  
  
10. Nel **Opzioni DNS** pagina, selezionare il **delega DNS aggiornare** casella di controllo se si desidera aggiornare la delega DNS durante l'installazione del ruolo e quindi fare clic su **Avanti**.  
  
11. Nel **Opzioni aggiuntive** pagina, digitare o selezionare i percorsi di volume e della cartella per il file di database, i file di registro del servizio directory e file di sistema di volume (SYSVOL). Specificare le opzioni di replica in base alle esigenze e quindi fare clic su **Avanti**.  
  
12. Nel **Verifica opzioni** pagina, esaminare le opzioni di installazione e quindi fare clic su **Avanti**.  
  
13. Nel **controllo dei prerequisiti** pagina, dopo aver convalidati i prerequisiti, fare clic su **installare**.  
  
14. Attendere fino al termine della procedura guidata di configurazione e quindi fare clic su **Chiudi**.  
  
15. Riavviare il computer se non è stato automaticamente riavviato.  
  
## <a name="BKMK_ConfigSG"></a>2,3. Configurare i gruppi di sicurezza  
Una distribuzione multisito richiede un gruppo di sicurezza aggiuntive per i computer client Windows 7 per ogni punto di ingresso nella distribuzione che consente di accedere ai computer client Windows 7. Se sono presenti più domini contenenti computer client Windows 7, è consigliabile creare un gruppo di sicurezza in ogni dominio per lo stesso punto di ingresso. In alternativa, può essere utilizzato un gruppo di protezione universale contenente i computer client da entrambi i domini. Ad esempio, in un ambiente con due domini, se si desidera consentire l'accesso al computer client Windows 7 in punti di ingresso 1 e 3, ma non nella voce punto 2, quindi creare due nuovi gruppi di protezione che contenga i computer client Windows 7 per ogni punto di ingresso in ognuno dei domini.  
  
### <a name="to-configure-additional-security-groups"></a>Per configurare i gruppi di sicurezza aggiuntive  
  
1.  Nel controller di dominio primario, fare clic su **avviare**, quindi fare clic su **Active Directory Users and Computers**.  
  
2.  Nell'albero della console, fare clic sulla cartella in cui si desidera aggiungere un nuovo gruppo, ad esempio, corp.contoso.com/Users. Scegliere **nuovo**, quindi fare clic su **gruppo**.  
  
3.  Nel **nuovo oggetto - gruppo** nella finestra di dialogo **nome gruppo**, digitare il nome del nuovo gruppo, ad esempio, Win7_Clients_Entrypoint1.  
  
4.  In **ambito del gruppo**, fare clic su **universale**, in **tipo di gruppo**, fare clic su **sicurezza**, quindi fare clic su **OK**.  
  
5.  Per aggiungere computer al nuovo gruppo di sicurezza, fare doppio clic sul gruppo di sicurezza e il **< nome_gruppo > proprietà** nella finestra di dialogo fare clic su di **membri** scheda.  
  
6.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
7.  Selezionare i computer Windows 7 per aggiungere a questo gruppo di sicurezza e quindi fare clic su **OK**.  
  
8.  Ripetere questa procedura per creare un gruppo di sicurezza per ogni punto di ingresso in base alle esigenze.  
  
![](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Per installare la funzionalità di Windows "Modulo Active Directory per Windows PowerShell":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
oppure aggiungere "Active Directory PowerShell Snap-In" tramite OptionalFeatures.  
  
Se in esecuzione i seguenti cmdlet di Windows Server 2008 R2 o Windows 7", è necessario importare il modulo di PowerShell per Active Directory:  
  
```  
Import-Module ActiveDirectory  
```  
  
Per configurare un gruppo di sicurezza denominato Win7_Clients_Entrypoint1 e per aggiungere un computer client denominato CLIENT2:  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="ConfigGPOs"></a>2,4. Configurare gli oggetti Criteri di gruppo  
Una distribuzione di accesso remoto multisito richiede gli oggetti Criteri di gruppo seguente:  
  
-   Un oggetto Criteri di gruppo per ogni punto di ingresso per il server di accesso remoto.  
  
-   Un oggetto Criteri di gruppo per tutti i computer client Windows 8 per ogni dominio.  
  
-   Un oggetto Criteri di gruppo in ogni dominio che contiene i computer client Windows 7 per ogni punto di ingresso configurato per il supporto di Windows 7 client.  
  
    > [!NOTE]  
    > Se non si dispongono di tutti i computer client Windows 7, non è necessario creare oggetti Criteri di gruppo per Windows 7 i computer.  
  
Quando si configura accesso remoto, la procedura guidata crea automaticamente gli oggetti Criteri di gruppo necessari se essi non "t esiste già. Se non si dispone delle autorizzazioni necessarie per creare oggetti Criteri di gruppo, deve essere creati prima di configurare accesso remoto. L'amministratore di DirectAccess deve disporre delle autorizzazioni complete su oggetti Criteri di gruppo (modifica + modifica sicurezza + CANC).  
  
> [!IMPORTANT]  
> Dopo la creazione manuale di oggetti Criteri di gruppo per l'accesso remoto è necessario consentire tempo sufficiente per Active Directory e replica DFS per il controller di dominio nel sito di Active Directory che viene associato al server di accesso remoto. Se accesso remoto creato automaticamente gli oggetti Criteri di gruppo, non sarà necessario alcun tempo di attesa.  
  
Per creare oggetti Criteri di gruppo, vedere [creare e modificare un oggetto Criteri di gruppo](https://technet.microsoft.com/library/cc754740.aspx).  
  
### <a name="DCMaintandDowntime"></a>Manutenzione e tempi di inattività del controller di dominio  
Quando un controller di dominio dell'emulatore PDC o controller di dominio, la gestione di oggetti Criteri di gruppo di server verificarsi tempi di inattività, non è possibile caricare o modificare la configurazione di accesso remoto. Questa operazione non influenza la connettività dei client se sono disponibili altri controller di dominio.  
  
Per caricare o modificare la configurazione di accesso remoto, è possibile trasferire il ruolo emulatore PDC a un controller di dominio diverso per il server di applicazione client o di oggetti Criteri di gruppo; per oggetti Criteri di gruppo di server, modificare i controller di dominio di gestire il server di oggetti Criteri di gruppo.  
  
> [!IMPORTANT]  
> Questa operazione può essere eseguita solo da un amministratore di dominio. L'impatto della modifica del controller di dominio primario non è confinato a accesso remoto; Pertanto, prestare attenzione durante il trasferimento del ruolo di emulatore PDC.  
  
> [!NOTE]  
> Prima di modificare l'associazione di controller di dominio, assicurarsi che tutti i GPO in distribuzione di accesso remoto sono stati replicati in tutti i controller di dominio nel dominio. Se l'oggetto Criteri di gruppo non è sincronizzata, le modifiche di configurazione recenti vadano perse dopo la modifica delle associazioni di controller di dominio, che potrebbero causare una configurazione danneggiata. Per verificare la sincronizzazione di oggetti Criteri di gruppo, vedere [stato dell'infrastruttura Criteri di gruppo verificare](https://technet.microsoft.com/library/jj134176.aspx).  
  
#### <a name="TransferPDC"></a>Per trasferire il ruolo emulatore PDC  
  
1.  Nel **avviare** digitare**DSA. msc**, quindi premere INVIO.  
  
2.  Nel riquadro sinistro della console di Active Directory Users and Computers, fare doppio clic su **Active Directory Users and Computers**, quindi fare clic su **Cambia Controller di dominio**. Nella finestra di dialogo Modifica Server di Directory, fare clic su **questo Controller di dominio o istanza AD LDS**, nell'elenco fare clic su controller di dominio che il nuovo proprietario del ruolo e quindi fare clic su **OK**.  
  
    > [!NOTE]  
    > Se non sono nel controller di dominio a cui si desidera trasferire il ruolo, è necessario eseguire questo passaggio. Non eseguire questo passaggio se si è già connessi al controller di dominio a cui si desidera trasferire il ruolo.  
  
3.  Nell'albero della console fare doppio clic su **Active Directory Users and Computers**, scegliere **tutte le attività**, quindi fare clic su **master operazioni**.  
  
4.  Nella finestra di dialogo master operazioni, fare clic sui **PDC** scheda e quindi fare clic su **Modifica**.  
  
5.  Fare clic su **Sì** per confermare che si desidera trasferire il ruolo e quindi fare clic su **Chiudi**.  
  
#### <a name="ChangeDC"></a>Per modificare il controller di dominio che gestisce gli oggetti Criteri di gruppo del server  
  
-   Eseguire il cmdlet Windows PowerShell  `HYPERLINK "https://technet.microsoft.com/library/hh918412.aspx" Set-DAEntryPointDC` sul server di accesso remoto e specificare il nome del controller di dominio non è raggiungibile per i *ExistingDC* parametro. Questo comando Modifica l'associazione di controller di dominio per il server di oggetti Criteri di gruppo dei punti di ingresso che sono attualmente gestiti dal controller di dominio.  
  
    -   Per sostituire il controller di dominio non raggiungibile "DC1" con il controller di dominio "DC2", eseguire le operazioni seguenti:  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   Per sostituire il controller di dominio non raggiungibile "DC1" con un controller di dominio nel sito di Active Directory più vicino al server di accesso remoto "DA1.corp.contoso.com", eseguire le operazioni seguenti:  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="ChangeTwoDCs"></a>Modificare due o più controller di dominio che gestiscono oggetti Criteri di gruppo del server  
In un numero minimo di casi, non sono disponibili due o più controller di dominio che gestiscono oggetti Criteri di gruppo di server. In questo caso, sono necessari ulteriori passaggi per modificare l'associazione di controller di dominio per il server di oggetti Criteri di gruppo.  
  
Informazioni sull'associazione di controller di dominio vengono archiviati sia nel Registro di sistema del server di accesso remoto e in tutti i GPO di server. Nell'esempio seguente, esistono due punti di ingresso con due server di accesso remoto, "DA1" in "punto di ingresso 1" e "DA2" in "Punto di ingresso 2". Il server oggetto Criteri di gruppo "Punto di ingresso 1" è gestito nel controller di dominio "DC1", mentre il server oggetto Criteri di gruppo del "punto di ingresso 2" è gestito nel controller di dominio "DC2". "DC1" e "DC2" non sono disponibili. È ancora disponibile un terzo controller di dominio nel dominio, "DC3", e i dati da "DC1" e "DC2" è stato già replicati "DC3".  
  
![Configurare l'infrastruttura multisito](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>Per modificare due o più controller di dominio che gestiscono oggetti Criteri di gruppo di server  
  
1.  Per sostituire il controller di dominio non disponibile "DC2" con il controller di dominio "DC3", eseguire il comando seguente:  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Questo comando Aggiorna il dominio controller associazione per il server di "Punto di ingresso 2" oggetto Criteri di gruppo nel Registro di sistema di DA2 e nel server "Punto di ingresso 2" oggetto Criteri di gruppo. Tuttavia, non viene aggiornato il server oggetto Criteri di gruppo "Punto di ingresso 1" perché non è disponibile il controller di dominio che lo gestisce.  
  
    > [!TIP]  
    > Questo comando utilizza il valore di continuazione per il *ErrorAction* parametro, che aggiorna il server di "Punto di ingresso 2" GPO nonostante l'impossibilità di aggiornare il server "1 punto di ingresso" oggetto Criteri di gruppo.  
  
    La configurazione risultante è illustrata nel diagramma seguente.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  Per sostituire il controller di dominio non disponibile "DC1" con il controller di dominio "DC3", eseguire il comando seguente:  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Questo comando Aggiorna l'associazione di controller di dominio per il server di server oggetto Criteri di gruppo nel Registro di sistema di DA1 e in "1 punto di ingresso" e "Punto di ingresso 2" di "Punto di ingresso 1" oggetti Criteri di gruppo. La configurazione risultante è illustrata nel diagramma seguente.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  Per sincronizzare l'associazione di controller di dominio per il server di "Punto di ingresso 2" oggetto Criteri di gruppo nel server "1 punto di ingresso" criteri di gruppo, eseguire il comando per sostituire "DC2" con "DC3", quindi specificare il server di accesso remoto, il cui oggetto Criteri di gruppo di server non è sincronizzata, in questo caso "DA1", per il *nomecomputer* parametro.  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    La configurazione finale viene illustrata nel diagramma seguente.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="ConfigDistOptimization"></a>Ottimizzazione della distribuzione della configurazione  
Quando si apportano modifiche di configurazione, le modifiche vengono applicate solo dopo che il server di oggetti Criteri di gruppo propagazione al server di accesso remoto. Per ridurre il tempo di distribuzione della configurazione, accesso remoto seleziona automaticamente un controller di dominio scrivibile che è il collegamento ipertestuale "<https://technet.microsoft.com/library/cc978016.aspx>" più vicino al server di accesso remoto durante la creazione dell'oggetto Criteri di gruppo del server.  
  
In alcuni scenari, può essere necessario modificare manualmente il controller di dominio che gestisce un oggetto Criteri di gruppo di server per ottimizzare i tempi di distribuzione di configurazione:  
  
-   Non si sono nessun controller di dominio scrivibile nel sito di Active Directory di un server di accesso remoto al momento dell'aggiunta del modello come un punto di ingresso. A questo punto viene aggiunto un controller di dominio scrivibile per il sito di Active Directory del server di accesso remoto.  
  
-   Un cambio di indirizzo IP o una modifica della subnet e siti di Active Directory potrebbe essere spostato il server di accesso remoto a un sito di Active Directory diverso.  
  
-   L'associazione di controller di dominio per un punto di ingresso è stato modificato manualmente per interventi di manutenzione su un controller di dominio e ora il controller di dominio è tornata in linea.  
  
In questi scenari, eseguire il cmdlet PowerShell `Set-DAEntryPointDC` sul server di accesso remoto e specificare il nome del punto di ingresso che si desidera ottimizzare l'utilizzo del parametro *EntryPointName*. Eseguire questa operazione solo dopo che i dati oggetto Criteri di gruppo dal controller di dominio attualmente archiviando il server che oggetto Criteri di gruppo è stato già replicato nel nuovo controller di dominio desiderato.  
  
> [!NOTE]  
> Prima di modificare l'associazione di controller di dominio, assicurarsi che tutti i GPO in distribuzione di accesso remoto sono stati replicati in tutti i controller di dominio nel dominio. Se l'oggetto Criteri di gruppo non è sincronizzata, le modifiche di configurazione recenti vadano perse dopo la modifica delle associazioni di controller di dominio, che potrebbero causare una configurazione danneggiata. Per verificare la sincronizzazione di oggetti Criteri di gruppo, vedere [stato dell'infrastruttura Criteri di gruppo verificare](https://technet.microsoft.com/library/jj134176.aspx).  
  
Per ottimizzare i tempi di distribuzione della configurazione, effettuare una delle seguenti operazioni:  
  
-   Per gestire il server oggetto Criteri di gruppo della voce punto "voce 1" in un controller di dominio nel sito di Active Directory più vicino al server di accesso remoto "DA1.corp.contoso.com", eseguire il comando seguente:  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   Per gestire il server oggetto Criteri di gruppo della voce punto "voce 1" nel dominio controller "DC2", eseguire il comando seguente:  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > Quando si modifica il controller di dominio associato a un punto di ingresso specifico, è necessario specificare un server di accesso remoto che è un membro di tale punto di ingresso per il *nomecomputer* parametro.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 3: configurare la distribuzione multisito](Step-3-Configure-the-Multisite-Deployment.md)  
-   [Passaggio 1: implementare una distribuzione di accesso remoto a server singolo](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  

