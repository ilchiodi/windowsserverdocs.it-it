---
title: route_ws2008
description: Informazioni su come modificare e visualizzare le voci nella tabella di routing IP locale.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afcd666c-0cef-47c2-9bcc-02d202b983b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: cc68dd5634ae4832376924c1678dc10a0427f2b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371419"
---
# <a name="route_ws2008"></a>route_ws2008

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza e modifica le voci nella tabella di routing IP locale. Viene usato senza parametri, **Route** Visualizza la guida.   

## <a name="syntax"></a>Sintassi  
```  
route [/f] [/p] [<Command> [<Destination>] [mask <Netmask>] [<Gateway>] [metric <Metric>]] [if <Interface>]]  
```  

### <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|/f|Cancella la tabella di routing di tutte le voci che non sono route host (route con una netmask di 255.255.255.255), la route di rete di loopback (route con una destinazione 127.0.0.0 e una netmask di 255.0.0.0) o una route multicast (route con una destinazione 224.0.0.0 e una netmask di 240.0.0.0). Se viene usato insieme a uno dei comandi (ad esempio, Aggiungi, modifica o Elimina), la tabella viene cancellata prima di eseguire il comando.|  
|/ p|Quando viene usato con il comando Add, la route specificata viene aggiunta al registro di sistema e viene usata per inizializzare la tabella di routing IP ogni volta che viene avviato il protocollo TCP/IP. Per impostazione predefinita, le route aggiunte non vengono mantenute quando viene avviato il protocollo TCP/IP. Quando viene usato con il comando stampa, viene visualizzato l'elenco delle route permanenti. Questo parametro viene ignorato per tutti gli altri comandi. Le route permanenti vengono archiviate nel percorso del registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\PersistentRoutes**.|  
|\<Command >|Specifica il comando che si desidera eseguire. Nella tabella seguente sono elencati i comandi validi:<br /><br />-   **Add:** aggiunge una route.<br />modifica -    **:** modifica una route esistente.<br />-   **Delete:** Elimina una route o route.<br />stampa -    **:** stampa una route o route.|  
|\<Destination >|Specifica la destinazione di rete della route. La destinazione può essere un indirizzo IP di rete (in cui i bit host dell'indirizzo di rete sono impostati su 0), un indirizzo IP per una route host o 0.0.0.0 per la route predefinita.|  
|maschera \<Netmask >|Specifica la destinazione di rete della route. La destinazione può essere un indirizzo IP di rete (in cui i bit host dell'indirizzo di rete sono impostati su 0), un indirizzo IP per una route host o 0.0.0.0 per la route predefinita.|  
|\<Gateway >|Specifica l'indirizzo IP dell'hop successivo o successivo in cui è raggiungibile il set di indirizzi definito dalla destinazione di rete e subnet mask. Per le route di subnet collegate localmente, l'indirizzo del gateway è l'indirizzo IP assegnato all'interfaccia associata alla subnet. Per le route remote, disponibile in uno o più router, l'indirizzo del gateway è un indirizzo IP direttamente raggiungibile assegnato a un router adiacente.|  
|metrica \<Metric >|Specifica una metrica di costo Integer (da 1 a 9999) per la route, che viene utilizzata quando si sceglie tra più route nella tabella di routing che corrispondono maggiormente all'indirizzo di destinazione di un pacchetto inoltrato. Viene scelta la route con la metrica più bassa. La metrica può riflettere il numero di hop, la velocità del tracciato, l'affidabilità del percorso, la velocità effettiva del percorso o le proprietà amministrative.|  
|Se \<Interface >|Specifica l'indice di interfaccia per l'interfaccia su cui è raggiungibile la destinazione. Per un elenco di interfacce e degli indici di interfaccia corrispondenti, usare la visualizzazione del comando di stampa della route. Per l'indice dell'interfaccia è possibile utilizzare valori decimali o esadecimali. Per i valori esadecimali, precedere il numero esadecimale con 0x. Quando il parametro if viene omesso, l'interfaccia viene determinata dall'indirizzo del gateway.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
- I valori di grandi dimensioni nella colonna **metrica** della tabella di routing sono i risultati che consentono a TCP/IP di determinare automaticamente la metrica per le route nella tabella di routing in base alla configurazione dell'indirizzo IP, della subnet mask e del gateway predefinito per ogni interfaccia LAN. La determinazione automatica della metrica dell'interfaccia, abilitata per impostazione predefinita, determina la velocità di ogni interfaccia e regola le metriche delle route per ogni interfaccia in modo che l'interfaccia più veloce crei le route con la metrica più bassa. Per rimuovere le metriche di grandi dimensioni, disabilitare la determinazione automatica della metrica dell'interfaccia dalle proprietà avanzate del protocollo TCP/IP per ogni connessione LAN.  
- I nomi possono essere usati per la *destinazione* se esiste una voce appropriata nel file di reti locali archiviato nella cartella <strong>systemroot\System32\Drivers @ no__t-2 e</strong>così via. È possibile usare i nomi per il *gateway* purché possano essere risolti in un indirizzo IP tramite tecniche standard di risoluzione dei nomi host, ad esempio query di Domain Name System (DNS), uso del file hosts locale archiviato in <strong>Systemroot\System32\Drivers @ no__t-2</strong> e la risoluzione dei nomi NetBIOS.  
- Se il comando è **Print** o **Delete**, è possibile omettere il parametro del *gateway* e usare i caratteri jolly per la destinazione e il gateway. Il valore di *destinazione* può essere un valore jolly specificato da un asterisco (*). Se la destinazione specificata contiene un asterisco (\*) o un punto interrogativo (?), viene considerato come un carattere jolly e vengono stampate o eliminate solo le route di destinazione corrispondenti. L'asterisco corrisponde a qualsiasi stringa e il punto interrogativo corrisponde a qualsiasi carattere singolo. Ad esempio, 10. \*,1, 192,168. \*, 127. \* e \*224 @ no__t-4 sono tutti usi validi del carattere jolly asterisco.  
- Se si utilizza una combinazione non valida di un valore di destinazione e di un subnet mask (netmask), viene visualizzato il messaggio di errore "Route: Indirizzo Mask Gateway errato". Questo messaggio di errore viene visualizzato quando la destinazione contiene uno o più bit impostati su 1 in posizioni dei bit in cui il subnet mask bit corrispondente è impostato su 0. Per testare questa condizione, esprimere la destinazione e subnet mask usando la notazione binaria. Il subnet mask in notazione binaria è costituito da una serie di 1 bit, che rappresenta la parte dell'indirizzo di rete della destinazione e da una serie di 0 bit, che rappresenta la parte dell'indirizzo host della destinazione. Controllare per determinare se nella destinazione sono presenti bit impostati su 1 per la parte della destinazione che rappresenta l'indirizzo host (come definito dall'subnet mask).  
- Il **/p** parametro è supportato solo per il comando route per windows NT 4,0, Windows 2000, Windows Millennium Edition, Windows XP e windows Server 2003. Questo parametro non è supportato dal comando **Route** per Windows 95 o Windows 98.  
- Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.  

## <a name="BKMK_Examples"></a>Esempi  
Per visualizzare l'intero contenuto della tabella di routing IP, digitare:  
```  
route print  
```  
Per visualizzare le route nella tabella di routing IP che iniziano con 10, digitare:  
```  
route print 10.*  
```  
Per aggiungere una route predefinita con l'indirizzo del gateway predefinito 192.168.12.1, digitare:  
```  
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1  
```  
Per aggiungere una route al 10.41.0.0 di destinazione con il subnet mask di 255.255.0.0 e l'indirizzo hop successivo di 10.27.0.1, digitare:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Per aggiungere una route persistente al 10.41.0.0 di destinazione con il subnet mask di 255.255.0.0 e l'indirizzo hop successivo di 10.27.0.1, digitare:  
```  
route /p add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Per aggiungere una route al 10.41.0.0 di destinazione con il subnet mask di 255.255.0.0, l'indirizzo hop successivo di 10.27.0.1 e la metrica di costo di 7, digitare:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 metric 7  
```  
Per aggiungere una route al 10.41.0.0 di destinazione con il subnet mask di 255.255.0.0, l'indirizzo hop successivo di 10.27.0.1 e l'uso dell'indice di interfaccia 0x3, digitare:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 if 0x3  
```  
Per eliminare la route per il 10.41.0.0 di destinazione con il subnet mask di 255.255.0.0, digitare:  
```  
route delete 10.41.0.0 mask 255.255.0.0  
```  
Per eliminare tutte le route nella tabella di routing IP che iniziano con 10, digitare:  
```  
route delete 10.*  
```  
Per modificare l'indirizzo hop successivo della route con la destinazione 10.41.0.0 e il subnet mask di 255.255.0.0 da 10.27.0.1 a 10.27.0.25, digitare:  
```  
route change 10.41.0.0 mask 255.255.0.0 10.27.0.25  
```  

## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
