---
title: Creare criteri di sicurezza con elenchi di controllo di accesso estesi per le porte
description: In questo argomento vengono fornite informazioni sugli elenchi di controllo di accesso (ACL) di porta estesi in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-hv-switch
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e61c3-f7d4-4e42-8575-79d75d05a218
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2beb4ec1d78200b5c62d18ffb3f935843bd12ae0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308023"
---
# <a name="create-security-policies-with-extended-port-access-control-lists"></a>Creare criteri di sicurezza con elenchi di controllo di accesso estesi per le porte

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni sugli elenchi di controllo di accesso (ACL) di porta estesi in Windows Server 2016. È possibile configurare ACL estesi nel commutatore virtuale Hyper-V in modo da consentire e bloccare il traffico di rete indirizzato e proveniente dalle macchine virtuali connesse al commutatore tramite schede di rete virtuali.  
  
In questo argomento sono contenute le seguenti sezioni.  
  
-   [Regole ACL dettagliate](#bkmk_detailed)  
  
-   [Regole ACL con stato](#bkmk_stateful)  
  
## <a name="detailed-acl-rules"></a><a name="bkmk_detailed"></a>Regole ACL dettagliate  
Commutatore virtuale Hyper-V gli ACL estesi consente di creare regole dettagliate che è possibile applicare alle singole schede di rete VM connesse al commutatore virtuale Hyper-V. La possibilità di creare regole dettagliate consente alle aziende e provider di servizi Cloud (CSP) per affrontare le minacce di protezione di rete in un ambiente server condiviso multi-tenant.  
  
Con gli ACL estesi è ora possibile bloccare o consentire il traffico di rete di singoli protocolli in esecuzione nelle macchine virtuali, anziché creare regole più vaste che bloccano o consentono il traffico di tutti i protocolli diretto o proveniente da una macchina virtuale. È possibile creare regole ACL estese in Windows Server 2016 che includono il set di parametri 5 tuple seguente: indirizzo IP, indirizzo IP di destinazione, protocollo, porta di origine e porta di destinazione di origine. Ogni regola inoltre può specificare la direzione del traffico di rete (in entrata o in uscita) e l'azione supportata dalla regola (blocco o abilitazione del traffico).  
  
È ad esempio possibile configurare gli ACL delle porte in modo che una macchina virtuale consenta tutto il traffico HTTP e HTTPS in entrata e in uscita sulla porta 80, bloccando invece il traffico di rete di tutti gli altri protocolli su tutte le porte.  
  
La possibilità di designare il traffico dei protocolli che può o non può essere ricevuto da macchine virtuali tenant garantisce flessibilità nella configurazione dei criteri di sicurezza.  
  
### <a name="configuring-acl-rules-with-windows-powershell"></a>Configurazione di regole ACL con Windows PowerShell  
Per configurare un ACL esteso, è necessario utilizzare il comando di Windows PowerShell **Add-VMNetworkAdapterExtendedAcl**. Questo comando supporta quattro sintassi diverse, con un utilizzo specifico per ogni sintassi:  
  
1.  Aggiungere un ACL esteso a tutte le schede di rete di una macchina Virtuale denominata - specificata dal primo parametro, - Name. Sintassi:  
  
    > [!NOTE]  
    > Se si desidera aggiungere un ACL esteso a una scheda di rete anziché a tutti, è possibile specificare la scheda di rete con il parametro - VMNetworkAdapterName.  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMName] <string[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny}  
        [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
2.  Aggiungere un ACL esteso a una scheda di rete virtuale specifica in una macchina virtuale. Sintassi:  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMNetworkAdapter] <VMNetworkAdapterBase[]> [-Action]  
        <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound |  
        Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort]  
        <string>] [[-Protocol] <string>] [-Weight] <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID  
        <int>] [-Passthru] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
3.  Aggiungere un ACL esteso a tutte le schede di rete virtuali riservate per l'utilizzo da parte del sistema operativo di gestione host Hyper-V.  
  
    > [!NOTE]  
    > Se si desidera aggiungere un ACL esteso a una scheda di rete anziché a tutti, è possibile specificare la scheda di rete con il parametro - VMNetworkAdapterName.  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction]  
        <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress]  
        <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight] <int> -ManagementOS  
        [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName <string>]  
        [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
4.  Aggiungere un ACL esteso a un oggetto macchina Virtuale che è stato creato in Windows PowerShell, ad esempio **$vm = get-vm "macchina_virtuale"** . Nella riga di codice successiva è possibile eseguire questo comando per creare un ACL esteso con la sintassi seguente:  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VM] <VirtualMachine[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow |  
        Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
### <a name="detailed-acl-rule-examples"></a>Esempi di regole ACL dettagliate  
Vengono riportati di seguito alcuni esempi di come utilizzare il comando **Add-VMNetworkAdapterExtendedAcl** per la configurazione di ACL delle porte estesi e per la creazione di criteri di sicurezza per le macchine virtuali.  
  
-   [Applicare la sicurezza a livello di applicazione](#bkmk_enforce)  
  
-   [Applicare la sicurezza a livello di utente e a livello di applicazione](#bkmk_both)  
  
-   [Fornire supporto per la sicurezza a un'applicazione non TCP/UDP](#bkmk_tcp)  
  
> [!NOTE]  
> I valori del parametro di regola **Direction** nelle tabelle riportate di seguito sono basati sul flusso di traffico diretto o proveniente dalla macchina virtuale per cui si desidera creare la regola. Se la macchina virtuale riceve traffico, il traffico è in entrata. Se invece invia traffico, il traffico è in uscita. Se ad esempio si applica a una macchina virtuale una regola che blocca il traffico in entrata, la direzione del traffico in entrata sarà dalle risorse esterne verso la macchina virtuale. Se si applica una regola che blocca il traffico in uscita, la direzione del traffico in uscita sarà dalla macchina virtuale locale verso le risorse esterne.  
  
### <a name="enforce-application-level-security"></a><a name="bkmk_enforce"></a>Applicare la sicurezza a livello di applicazione  
Poiché in molti server applicazioni vengono utilizzate porte TCP/UDP standardizzate per comunicare con i computer client, è facile creare regole che bloccano o consentono l'accesso a un server applicazioni filtrando il traffico diretto e proveniente dalla porta designata per l'applicazione.  
  
È ad esempio possibile consentire a un utente di eseguire l'accesso a un server applicazioni nel centro dati utilizzando Connessione Desktop remoto (RDP). Poiché il protocollo RDP utilizza la porta TCP 3389, è possibile configurare rapidamente la regola seguente:  
  
|IP origine|IP destinazione|Protocollo|Porta di origine|Porta di destinazione|Direzione|Azione|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|TCP|*|3389|Verso l'interno|Consenti|  
  
Vengono riportati di seguito due esempi di creazione di regole con i comandi di Windows PowerShell. La prima regola di esempio blocca tutto il traffico alla macchina Virtuale denominata "ApplicationServer". La seconda regola di esempio, viene applicata alla scheda di rete della macchina Virtuale denominata "ApplicationServer", consente solo il traffico RDP in ingresso alla macchina Virtuale.  
  
> [!NOTE]  
> Quando si creano regole, è possibile utilizzare il **-peso** parametro per determinare l'ordine in cui il commutatore virtuale Hyper-V elabora le regole. I valori del parametro **-peso** sono espressi come numeri interi; le regole con un numero più alto vengono elaborate prima delle regole con numeri più bassi. Se ad esempio sono state applicate due regole a una scheda di rete di macchina virtuale, una con parametro -weight impostato su 1 e un'altra con parametro -Weight impostato su 10, verrà applicata per prima la regola il cui parametro -Weight è impostato su 10.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Inbound" -LocalPort 3389 -Protocol "TCP" -Weight 10  
```  
  
### <a name="enforce-both-user-level-and-application-level-security"></a><a name="bkmk_both"></a>Applicare la sicurezza a livello di utente e a livello di applicazione  
Poiché può soddisfare un pacchetto IP di 5 tuple (IP origine, IP di destinazione, Protocollo, Porta di origine e Porta di destinazione), una regola consente di applicare criteri di sicurezza più dettagliati rispetto a un ACL delle porte.  
  
Ad esempio, se si desidera fornire il servizio DHCP a un numero limitato di client computer utilizzando un set specifico di server DHCP, è possibile configurare le regole seguenti nel computer Windows Server 2016 che esegue Hyper-V, in cui sono ospitate le macchine virtuali utente:  
  
|IP origine|IP destinazione|Protocollo|Porta di origine|Porta di destinazione|Direzione|Azione|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|255.255.255.255|UDP|*|67|Verso l'esterno|Consenti|  
|*|10.175.124.0/25|UDP|*|67|Verso l'esterno|Consenti|  
|10.175.124.0/25|*|UDP|*|68|Verso l'interno|Consenti|  
  
Vengono riportati di seguito esempi di creazione di queste regole con i comandi di Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 255.255.255.255 -RemotePort 67 -Protocol "UDP"-Weight 10  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 67 -Protocol "UDP"-Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 68 -Protocol "UDP"-Weight 20  
```  
  
### <a name="provide-security-support-to-a-non-tcpudp-application"></a><a name="bkmk_tcp"></a>Fornire supporto per la sicurezza a un'applicazione non TCP/UDP  
Mentre la maggior parte del traffico di rete in un centro dati è basato sui protocolli TCP e UDP, esiste comunque una parte di traffico basata su altri protocolli. Se ad esempio si desidera consentire a un gruppo di server di eseguire un'applicazione multicast IP basata sul protocollo IGMP (Internet Group Management Protocol), è possibile creare la regola seguente.  
  
> [!NOTE]  
> Il numero di protocollo IP designato per IGMP è 0x02.  
  
|IP origine|IP destinazione|Protocollo|Porta di origine|Porta di destinazione|Direzione|Azione|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|0x02|*|*|Verso l'interno|Consenti|  
|*|*|0x02|*|*|Verso l'esterno|Consenti|  
  
Viene riportato di seguito un esempio di creazione di queste regole con i comandi di Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -Protocol 2 -Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -Protocol 2 -Weight 20  
```  
  
## <a name="stateful-acl-rules"></a><a name="bkmk_stateful"></a>Regole ACL con stato  
Un'altra nuova funzionalità degli ACL estesi consente di configurare regole con stato. Una regola con stato Filtra i pacchetti in base a cinque attributi in un pacchetto - IP di origine, IP di destinazione, protocollo, porta di origine e porta di destinazione.  
  
Le regole con stato offrono le funzionalità seguenti:  
  
-   Consentono sempre il traffico e non vengono utilizzate per bloccare il traffico.  
  
-   Se si specifica che il valore del parametro **Direction** è in entrata e il traffico soddisfa la regola, il commutatore virtuale Hyper-V creerà dinamicamente una regola corrispondente per consentire alla macchina virtuale di inviare traffico in uscita in risposta alla risorsa esterna.  
  
-   Se invece si specifica che il valore del parametro **Direction** è in uscita e il traffico soddisfa la regola, il commutatore virtuale Hyper-V creerà dinamicamente una regola corrispondente per consentire al traffico in entrata della risorsa esterna di essere ricevuto dalla macchina virtuale.  
  
-   Includono un attributo di timeout misurato in secondi. Quando un pacchetto di rete che raggiunge il commutatore soddisfa una regola con stato, il commutatore virtuale Hyper-V crea uno stato in modo da consentire tutti i pacchetti successivi in entrambe le direzioni dello stesso flusso. Lo stato scade in assenza di traffico in una delle due direzioni nel periodo di tempo specificato dal valore di timeout.  
  
Viene riportato di seguito un esempio di utilizzo delle regole con stato.  
  
### <a name="allow-inbound-remote-server-traffic-only-after-it-is-contacted-by-the-local-server"></a>Consentire il traffico in entrata del server remoto solo dopo che è stato contattato dal server locale  
In alcuni casi è necessario utilizzare una regola con stato perché è l'unica soluzione in grado di tenere traccia di una connessione stabilita nota e di distinguere la connessione da altre connessioni.  
  
Se ad esempio si desidera consentire a un server applicazioni con macchina virtuale di avviare connessioni sulla porta 80 con servizi Web su Internet, e si desidera che i server Web remoti siano in grado di rispondere al traffico della macchina virtuale, è possibile configurare una regola con stato che consenta il traffico in uscita iniziale dalla macchina virtuale verso i servizi Web. Poiché viene utilizzata una regola con stato, è consentito anche il traffico di ritorno verso la macchina virtuale proveniente dai server Web. Per motivi di sicurezza, è possibile bloccare tutto il restante traffico di rete in entrata verso la macchina virtuale.  
  
Per definire questa configurazione di regola, è possibile utilizzare le impostazioni elencate nella tabella riportata di seguito.  
  
> [!NOTE]  
> Per le restrizioni di formattazione e la quantità di informazioni contenute nella tabella seguente, i dati vengono visualizzati in modo diverso rispetto alle tabelle precedenti di questo documento.  
  
|Parametro|Regola 1|Regola 2|Regola 3|  
|-------------|----------|----------|----------|  
|IP origine|*|*|*|  
|IP destinazione|*|*|*|  
|Protocollo|*|*|TCP|  
|Porta di origine|*|*|*|  
|Porta di destinazione|*|*|80|  
|Direzione|Verso l'interno|Verso l'esterno|Verso l'esterno|  
|Azione|Nega|Nega|Consenti|  
|Con stato|No|No|Sì|  
|Timeout (in secondi)|N/D|N/D|3600|  
  
La regola con stato consente al server applicazioni con macchina virtuale di connettersi a un server Web remoto. Quando viene inviato il primo pacchetto, il commutatore virtuale Hyper-V crea due stati di flusso per consentire tutti i pacchetti inviati al server Web remoto e tutti i pacchetti di ritorno. Quando il flusso di pacchetti tra i server si arresta, viene applicato il timeout degli stati di flusso secondo il valore designato di 3600 secondi, ovvero di un'ora.  
  
Viene riportato di seguito un esempio di creazione di queste regole con i comandi di Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1   
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Outbound" 80 "TCP" -Weight 100 -Stateful -Timeout 3600  
```  
  


