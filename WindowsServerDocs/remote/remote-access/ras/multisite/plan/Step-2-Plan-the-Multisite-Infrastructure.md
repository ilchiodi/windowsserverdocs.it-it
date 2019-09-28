---
title: Passaggio 2 piano infrastruttura multisito
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff8a58aa679691132d074ef52b876cea05366ab5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367099"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>Passaggio 2 piano infrastruttura multisito

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Il passaggio successivo della distribuzione di accesso remoto in una topologia multisita è per completare la pianificazione dell'infrastruttura multisito; tra cui Active Directory, gruppi di sicurezza e oggetti Criteri di gruppo.  
## <a name="bkmk_2_1_AD"></a>piano di 2,1 Active Directory  
Una distribuzione multisito può essere configurata in un numero di topologie di accesso remoto:  
  
-   **Singolo sito di Active Directory, più punti di ingresso**-In questa topologia, è necessario un singolo sito di Active Directory per l'intera organizzazione con collegamenti veloci intranet del sito, ma sono presenti più server di accesso remoto distribuito in tutta l'organizzazione, ogni che agisce come un punto di ingresso. Un esempio di questa topologia geografico è disporre di un singolo sito di Active Directory per gli Stati Uniti con punti di ingresso in costa orientale e costa occidentale.  
  
    ![Infrastruttura multisito](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **Più siti di Active Directory, più punti di ingresso**-In questa topologia, è necessario due o più siti di Active Directory con un server di accesso remoto distribuito come un punto di ingresso per ogni sito. Ogni server di accesso remoto è associato il controller di dominio Active Directory per il sito. Un esempio di questa topologia geografico consiste nel disporre di un sito di Active Directory per gli Stati Uniti e uno per l'Europa con un singolo punto di ingresso per ogni sito. Si noti che se si dispone di più siti di Active Directory non è necessario disporre di un punto di ingresso associato a ogni sito. Inoltre, alcuni siti di Active Directory possono avere più di un punto di ingresso è associato.  
  
    ![Topologia multisita](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
In un punto di ingresso multisito, è possibile configurare un singolo server di accesso remoto più server di accesso remoto, o cluster di server di accesso remoto.   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Indicazioni e procedure consigliate di active Directory  
Tenere presenti le indicazioni e vincoli per la distribuzione di Active Directory in uno scenario multisito:  
  
1.  Ogni sito di Active Directory può contenere uno o più server di accesso remoto o un cluster di server funziona come punti di ingresso multisito per i computer client. Un sito Active Directory non è tuttavia necessario disporre di un punto di ingresso.  
  
2.  Solo un punto di ingresso multisito può essere associato a un singolo sito di Active Directory. Quando i computer client che eseguono Windows 8 connettono a un punto di ingresso specifico, questi sono considerati come appartenenti al sito di Active Directory associato a tale punto di ingresso.  
  
3.  È consigliabile che ogni sito di Active Directory disponga di un controller di dominio. Il controller di dominio può essere di sola lettura.  
  
4.  Se ciascun sito Active Directory contiene un controller di dominio, l'oggetto Criteri di gruppo per un server nel punto di ingresso viene gestito da uno dei controller di dominio nel sito di Active Directory associato all'endpoint. Se non sono controller di dominio abilitato per la scrittura in tale sito, l'oggetto Criteri di gruppo per un server viene gestito in un controller di dominio abilitato per la scrittura che risulta più vicino al primo server di accesso remoto è configurato nel punto di ingresso. Più vicino è determinato da un collegamento di calcolo dei costi. Si noti che in questo scenario, dopo aver apportato modifiche alla configurazione potrebbero essere presenti un ritardo durante la replica tra controller di dominio oggetto Criteri di gruppo di gestione e il controller di dominio di sola lettura nel sito di Active Directory del server.  
  
5.  Oggetti Criteri di gruppo di client e server applicazioni facoltativo che GPO vengono gestiti nel controller di dominio in esecuzione dell'emulatore del Controller di dominio primario (PDC). Ciò significa che il client di oggetti Criteri di gruppo non sono necessariamente gestiti nel sito di Active Directory che contiene il punto di ingresso per i client in cui connettersi.  
  
6.  Se il controller di dominio per un sito di Active Directory non è raggiungibile, il server di accesso remoto si connetterà a un controller di dominio alternativo nel sito (se disponibile). In caso contrario, si connette al controller di dominio per un altro sito per recuperare un oggetto Criteri di gruppo aggiornato e per autenticare i client. In entrambi i casi, la console di gestione accesso remoto e i cmdlet di PowerShell non possono essere utilizzati per recuperare o modificare le impostazioni di configurazione fino a quando non è disponibile il controller di dominio. Tenere presente quanto segue:  
  
    1.  Se il server in esecuzione dell'emulatore PDC è disponibile, è necessario designare un controller di dominio che ha aggiornato oggetti Criteri di gruppo dell'emulatore PDC.  
  
    2.  Se il controller di dominio che gestisce un oggetto Criteri di gruppo di server è disponibile, utilizzare il cmdlet Set-DAEntryPointDC PowerShell per associare un nuovo controller di dominio con il punto di ingresso. Il nuovo controller di dominio deve disporre oggetti Criteri di gruppo aggiornato prima di eseguire il cmdlet.  
  
## <a name="bkmk_2_2_SG"></a>2,2 pianificare i gruppi di sicurezza  
Durante la distribuzione di un singolo server con impostazioni avanzate, sono stati raccolti tutti i computer client che accedono alla rete interna tramite DirectAccess in un gruppo di sicurezza. In una distribuzione multisito, viene utilizzato questo gruppo di sicurezza per Windows 8 solo i computer client. Per una distribuzione multisita, i computer client Windows 7 saranno raccolti nei gruppi di protezione distinti per ogni punto di ingresso nella distribuzione multisito. Ad esempio, se in precedenza sono raggruppati tutti i computer client nel gruppo di client_da, è necessario ora rimuovere tutti i computer Windows 7 da quel gruppo e inserirli in un altro gruppo di protezione. Ad esempio, in più siti di Active Directory, punti di ingresso più di topologia, si crea un gruppo di sicurezza per il punto di ingresso di Stati Uniti (DA_Clients_US) e uno per il punto di ingresso Europa (DA_Clients_Europe). Inserire tutti i computer client Windows 7 in United States nel gruppo di DA_Clients_US e qualsiasi Europa all'interno del gruppo DA_Clients_Europe. Se non si dispongono di tutti i computer client Windows 7, non è necessario pianificare i gruppi di sicurezza per i computer Windows 7.  
  
Gruppi di sicurezza necessari sono i seguenti:  
  
-   Un gruppo di protezione per tutti i computer client Windows 8. È consigliabile creare un gruppo di sicurezza univoco per questi client per ogni dominio.  
  
-   Un gruppo di sicurezza contenente i computer client Windows 7 per ogni punto di ingresso. È consigliabile creare un gruppo univoco per ogni dominio. Esempio: Domain1\DA_Clients_Europe; Domain2\DA_Clients_Europe; Domain1\DA_Clients_US; Domain2\DA_Clients_US.  
  
## <a name="bkmk_2_3_GPO"></a>2,3 pianificare oggetti Criteri di gruppo  
Impostazioni di DirectAccess configurate durante la distribuzione di accesso remoto vengono raccolti in oggetti Criteri di gruppo. La distribuzione a server singolo già utilizza oggetti Criteri di gruppo per i client DirectAccess, server di accesso remoto e, facoltativamente, per i server applicazioni. Una distribuzione multisito richiede i seguenti oggetti Criteri di gruppo:  
  
-   Un oggetto Criteri di gruppo di server per ogni punto di ingresso.  
  
-   Un oggetto Criteri di gruppo per ogni dominio contenente i computer client che eseguono Windows 8.  
  
-   Un oggetto Criteri di gruppo per ogni punto di ingresso e ogni dominio contenente i computer client Windows 7.  
  
Oggetti Criteri di gruppo può essere configurato come segue:  
  
-   **Automaticamente**-è possibile specificare che i GPO vengono creati automaticamente da accesso remoto. Un nome predefinito specificato per ogni oggetto Criteri di gruppo e può essere modificato.  
  
-   **Manualmente**-è possibile utilizzare oggetti Criteri di gruppo creati manualmente dall'amministratore di Active Directory.  
  
> [!NOTE]  
> Una volta DirectAccess è configurato per utilizzare oggetti Criteri di gruppo specifico, non è possibile configurare per l'utilizzo di diversi oggetti Criteri di gruppo.  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 creati automaticamente oggetti Criteri di gruppo  
Tenere presente quanto segue quando si usano oggetti Criteri di gruppo creati automaticamente:  
  
-   Gli oggetti Criteri di gruppo creati automaticamente vengono applicati secondo i parametri di percorso e destinazione collegamento, come segue:  
  
    -   Per il server oggetto Criteri di gruppo, parametri sia il percorso e collegamento puntano al dominio contenente il server di accesso remoto.  
  
    -   Per client oggetti Criteri di gruppo, la destinazione del collegamento è impostata per la radice del dominio in cui è stato creato l'oggetto Criteri di gruppo. Viene creato un oggetto Criteri di gruppo per ogni dominio che contiene i computer client e l'oggetto Criteri di gruppo verrà collegato alla radice di ogni dominio. .  
  
-   Criteri di gruppo creati automaticamente, per applicare le impostazioni di DirectAccess l'accesso remoto amministratore del server richiede le autorizzazioni seguenti:  
  
    -   Autorizzazioni per domini richiesti per la creazione oggetto Criteri di gruppo.  
  
    -   Autorizzazioni per il collegamento di tutte le radici del dominio client selezionate.  
  
    -   Autorizzazione per creare il filtro WMI per oggetti Criteri di gruppo è necessaria se DirectAccess è stato configurato per funzionare solo nei computer portatili.  
  
    -   Autorizzazioni per i collegamenti per le radici di domini associati i punti di ingresso (i domini di oggetto Criteri di gruppo di server)  
  
    -   È consigliabile che l'amministratore di Accesso remoto disponga delle autorizzazioni di lettura dell'oggetto Criteri di gruppo per ogni dominio richiesto. Questo consente l'accesso remoto verificare che non esistano oggetti Criteri di gruppo con nomi duplicati durante la creazione di oggetti Criteri di gruppo per la distribuzione multisita.  
  
    -   Replica le autorizzazioni Active Directory nei domini associati punti di ingresso. Questo avviene perché quando si aggiunge inizialmente punti di ingresso, il server oggetto Criteri di gruppo per il punto di ingresso viene creato nel controller di dominio più vicino a tale punto di ingresso. Tuttavia, poiché la creazione di collegamento è supportata solo nell'emulatore PDC, l'oggetto Criteri di gruppo devono essere replicate dal controller di dominio in cui è stato creato per il controller di dominio in esecuzione dell'emulatore PDC prima di creare il collegamento.  
  
Si noti che, se le autorizzazioni corrette per la replica e il collegamento di oggetti Criteri di gruppo non esiste, viene generato un avviso. L'operazione di accesso remoto continuerà ma la replica e il collegamento non verrà eseguito. Se un collegamento di avviso viene generato verranno creati i collegamenti non essere automaticamente, anche dopo che le autorizzazioni sono disponibili. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
### <a name="232-manually-created-gpos"></a>2.3.2 Creazione manuale oggetti Criteri di gruppo  
Tenere presente quanto segue quando si usano oggetti Criteri di gruppo creati manualmente:  
  
-   I seguenti oggetti Criteri di gruppo deve essere creato manualmente per la distribuzione multisita:  
  
    -   **Oggetto Criteri di gruppo**- un oggetto Criteri di gruppo server per ogni punto di ingresso (nel dominio in cui si trova il punto di ingresso). Questo oggetto Criteri di gruppo verrà applicato in ogni server di accesso remoto nel punto di ingresso.  
  
    -   **Oggetto Criteri di gruppo (Windows 7)** - un oggetto Criteri di gruppo per ogni punto di ingresso e ogni dominio contenente i computer client Windows 7 che si connetteranno ai punti di ingresso nella distribuzione multisito. Ad esempio Domain1\DA_W7_Clients_GPO_Europe; Domain2\DA_W7_Clients_GPO_Europe; Domain1\DA_W7_Clients_GPO_US; Domain2\DA_W7_Clients_GPO_US. Se nessuno dei computer client Windows 7 si connetterà ai punti di ingresso, oggetti Criteri di gruppo non sono necessari.  
  
-   Non è necessario creare altri oggetti Criteri di gruppo per Windows 8 client computer. Un oggetto Criteri di gruppo per ogni dominio contenente i computer client è già stato creato quando è stato distribuito il singolo server di accesso remoto. In una distribuzione multisito questi oggetti Criteri di gruppo client funzionerà come i client di oggetti Criteri di gruppo per Windows 8.  
  
-   Oggetti Criteri di gruppo deve essere disponibile prima di fare clic sul pulsante di conferma nelle procedure guidate di distribuzione multisito.  
  
-   Quando si usano oggetti Criteri di gruppo creati manualmente viene eseguita una ricerca di un collegamento all'oggetto Criteri di gruppo nell'intero dominio. Se l'oggetto Criteri di gruppo non è collegato nel dominio, verrà automaticamente creato un collegamento nel dominio radice. Se le autorizzazioni richieste per creare il collegamento non sono disponibili, verrà emesso un avviso.  
  
-   Quando tramite creati manualmente per applicare le impostazioni di DirectAccess l'amministratore del server Accesso remoto richiede l'oggetto Criteri di gruppo le autorizzazioni complete (modifica, eliminazione, modifica sicurezza) su oggetti Criteri di gruppo creati manualmente.  
  
Si noti che, se le autorizzazioni corrette per la replica e il collegamento di oggetti Criteri di gruppo non esiste, viene generato un avviso. L'operazione di accesso remoto continuerà ma la replica e il collegamento non verrà eseguito. Se un collegamento di avviso viene generato verranno creati i collegamenti non essere automaticamente, anche dopo che le autorizzazioni sono disponibili. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 gestiscono oggetti Criteri di gruppo in un ambiente multidominio controller  
Ogni oggetto Criteri di gruppo verranno gestiti da un controller di dominio specifico, come indicato di seguito:  
  
-   Il server oggetto Criteri di gruppo viene gestito da uno dei controller di dominio nel sito di Active Directory associati al server, o se i controller di dominio in tale sito sono di sola lettura, da un controller di dominio abilitato per la scrittura più vicino al primo server nella voce del punto.  
  
-   Oggetti Criteri di gruppo client verranno gestiti dal controller di dominio in esecuzione dell'emulatore PDC.  
  
Se si desidera manualmente modificare nota impostazioni oggetto Criteri di gruppo seguenti:  
  
-   Per il server oggetti Criteri di gruppo, per identificare il controller di dominio è associato a un punto di ingresso specifico, eseguire il cmdlet PowerShell `Get-DAEntryPointDC -EntryPointName <name of entry point>`.  
  
-   Per impostazione predefinita, quando si apportano modifiche con i cmdlet PowerShell di rete o la console Gestione criteri di gruppo, viene utilizzato il controller di dominio che agisce come l'emulatore PDC.  
  
-   Se si modificano le impostazioni in un controller di dominio che non è il controller di dominio associato il punto di ingresso (per server oggetti Criteri di gruppo) o l'emulatore PDC (per oggetti Criteri di gruppo client), tenere presente quanto segue:  
  
    1.  Prima di modificare le impostazioni, verificare che il controller di dominio viene replicato con un oggetto Criteri di gruppo aggiornato, e [Backup oggetto Criteri di gruppo impostazioni](https://go.microsoft.com/fwlink/?LinkID=257928), prima di apportare modifiche. Se l'oggetto Criteri di gruppo non viene aggiornata, merge durante la replica potrebbero verificarsi conflitti, risultante in una configurazione di accesso remoto danneggiata.  
  
    2.  Dopo aver modificato le impostazioni, è necessario attendere che le modifiche vengano replicate al controller di dominio che è associato a oggetti Criteri di gruppo. Non apportare modifiche aggiuntive utilizzando la console di gestione accesso remoto o i cmdlet PowerShell di accesso remoto fino al completamento della replica. Se un oggetto Criteri di gruppo viene modificato in due diversi controller di dominio prima del completamento della replica, merge potrebbero verificarsi conflitti, risultante in una configurazione danneggiata  
  
-   In alternativa è possibile modificare l'impostazione predefinita utilizzando il **Cambia Controller di dominio** finestra di dialogo casella nella console Gestione criteri di gruppo o tramite il **Open-NetGPO** cmdlet PowerShell, in modo che le modifiche apportate tramite la console o i cmdlet di rete utilizzano il controller di dominio specificato.  
  
    1.  A tale scopo nella console Gestione criteri di gruppo, fare doppio clic il contenitore di dominio o i siti e fare clic su **Cambia Controller di dominio**.  
  
    2.  A tale scopo in PowerShell, specificare il parametro DomainController per il cmdlet Open-NetGPO. Ad esempio, per abilitare i profili pubblici e privati in Windows Firewall in un oggetto Criteri di gruppo denominato domain1\DA_Server_GPO _ Europe usando un controller di dominio denominato dc.corp.contoso.com Europa, eseguire le operazioni seguenti:  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>Modifica associazione controller di dominio  
Per mantenere la coerenza di configurazione in una distribuzione multisito, è importante assicurarsi che ogni oggetto Criteri di gruppo sia gestito da un singolo controller di dominio. In alcuni scenari, può essere necessario assegnare un controller di dominio per un oggetto Criteri di gruppo:  
  
-   **Manutenzione di controller di dominio e i tempi di inattività**-quando il controller di dominio che gestisce un oggetto Criteri di gruppo non è disponibile, potrebbe essere richiesto per gestire l'oggetto Criteri di gruppo in un controller di dominio diverso.  
  
-   **Ottimizzazione della distribuzione della configurazione**-dopo l'infrastruttura di rete cambia, potrebbe essere necessario gestire il server oggetto Criteri di gruppo di un punto di ingresso in un controller di dominio nello stesso sito Active Directory come punto di ingresso.   
  
## <a name="bkmk_2_4_DNS"></a>2,4 pianificare il DNS  
Durante la pianificazione di DNS per una distribuzione multisito, tenere presente quanto segue:  
  
1.  I client utilizzano l'indirizzo ConnectTo per connettersi al server di accesso remoto. Ogni punto di ingresso della distribuzione richiede un diverso indirizzo ConnectTo. Ogni indirizzo ConnectTo punto di ingresso deve essere disponibile nel DNS pubblico e l'indirizzo scelto dall'utente deve corrispondere al nome soggetto del certificato IP-HTTPS distribuito per la connessione IP-HTTPS.   
  
2.  Inoltre, accesso remoto consente di applicare il routing simmetrica. Pertanto, solo i client Teredo e IP-HTTPS possono connettersi quando è abilitata una distribuzione multisito. Per consentire ai client IPv6 nativi per la connessione, sia l'esterno (con connessione Internet) indirizzi IPv6 e IPv4 nel server di accesso remoto deve essere risolto l'indirizzo ConnectTo (l'URL IP-HTTPS).  
  
3.  Accesso remoto crea un probe Web predefinito usato dai computer client DirectAccess per verificare la connettività alla rete interna. Durante la configurazione del server solo i nomi seguenti devono sono stati registrati in DNS:  
  
    1.  DirectAccess-WebProbeHost deve risolvere l'indirizzo IPv4 interno del server di accesso remoto o l'indirizzo IPv6 in un ambiente solo IPv6.  
  
    2.  DirectAccess-CorpConnectivityHost deve risolvere un indirizzo localhost (loopback) (sia:: 1 o 127.0.0.1, a seconda se IPv6 viene distribuito nella rete aziendale).  
  
    In una distribuzione multisito è necessario creare una voce DNS aggiuntiva per directaccess-WebProbeHost per ogni punto di ingresso. Quando si aggiunge un punto di ingresso, accesso remoto tenta di creare automaticamente questa voce di directaccess-WebProbeHost aggiuntive. Tuttavia, in caso contrario verrà visualizzato un messaggio di avviso ed è necessario creare la voce manualmente.  
  
    > [!NOTE]  
    > Quando è abilitato lo scavenging DNS nell'infrastruttura DNS, è consigliabile disabilitare lo scavenging sulle voci DNS create automaticamente da accesso remoto.  
  

  



