---
title: route_ws2008
description: Informazioni su come modificare e visualizzare le voci nella tabella di routing IP locale.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1164767a80b4d7dd24152bc34eda5d88834c1bdb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854822"
---
# <a name="routews2008"></a>route_ws2008

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza e modifica le voci nella tabella di routing IP locale. Se utilizzato senza parametri, **route** Visualizza la Guida.   

## <a name="syntax"></a>Sintassi  
```  
route [/f] [/p] [<Command> [<Destination>] [mask <Netmask>] [<Gateway>] [metric <Metric>]] [if <Interface>]]  
```  

### <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|/f|Cancella la tabella di routing di tutte le voci che non sono le route host (route con una netmask 255.255.255.255), la route di rete loopback (route con destinazione 127.0.0.0 e una subnet mask 255.0.0.0) o una route multicast (route con una destinazione di 224.0.0.0 e una subnet mask 240.0.0.0). Se viene utilizzato in combinazione con uno dei comandi (ad esempio come aggiungere, modificare o eliminare), la tabella viene cancellata prima di eseguire il comando.|  
|/ p|Se usato con il comando add, la route specificata viene aggiunto al Registro di sistema e viene usata per inizializzare la tabella di routing IP ogni volta che viene avviato il protocollo TCP/IP. Per impostazione predefinita, le route aggiunta non vengono mantenute quando viene avviato il protocollo TCP/IP. Se usato con il comando di stampa, viene visualizzato l'elenco delle route persistente. Questo parametro viene ignorato per tutti gli altri comandi. Le route permanenti vengono archiviate nel percorso del Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\PersistentRoutes**.|  
|\<Comando >|Specifica il comando da eseguire. Nella tabella seguente elenca i comandi validi:<br /><br />-   **Aggiungi:** aggiunge una route.<br />-   **modificare:** modifica una route esistente.<br />-   **Elimina:** consente di eliminare una o più route.<br />-   **stampare:** stampa una o più route.|  
|\<Destinazione >|Specifica la destinazione della route di rete. La destinazione può essere un indirizzo di rete IP (in cui i bit di host dell'indirizzo di rete vengono impostati su 0), un indirizzo IP per una route host o per la route predefinita 0.0.0.0.|  
|maschera \<Netmask >|Specifica la destinazione della route di rete. La destinazione può essere un indirizzo di rete IP (in cui i bit di host dell'indirizzo di rete vengono impostati su 0), un indirizzo IP per una route host o per la route predefinita 0.0.0.0.|  
|\<Gateway>|Specifica l'indirizzo IP hop di inoltro o successivo su cui il set di indirizzi definiti per la destinazione e una subnet mask di rete sono raggiungibili. Per le route della subnet collegata localmente, l'indirizzo del gateway è l'indirizzo IP assegnato all'interfaccia che viene collegata alla subnet. Per le route remote, disponibili tramite i router di uno o più, l'indirizzo del gateway è un indirizzo IP raggiungibile direttamente assegnato a un router adiacente.|  
|metrica \<metrica >|Specifica una metrica di costo di integer (compreso nell'intervallo da 1 a 9999) per la route, che viene usata quando si sceglie tra più route nella tabella di routing che meglio soddisfano l'indirizzo di destinazione di un pacchetto da inoltrare. La route con il valore metrico più basso viene scelto. La metrica può riflettere il numero di hop, la velocità del percorso, percorso affidabilità, velocità effettiva di percorso o le proprietà di amministrazione.|  
|Se \<interfaccia >|Specifica l'indice dell'interfaccia per l'interfaccia in cui la destinazione è raggiungibile. Per un elenco di interfacce e i relativi indici interfaccia corrispondente, utilizzare la visualizzazione del comando Stampa route. È possibile usare i valori decimali o esadecimali per l'indice dell'interfaccia. Per i valori esadecimali, anteporre al numero esadecimale 0x. Quando il se parametro viene omesso, l'interfaccia è determinato dall'indirizzo del gateway.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
-   Valori di grandi dimensioni nei **metrica** colonna della tabella di routing sono il risultato di consentire TCP/IP determinare automaticamente la metrica per le route nella tabella di routing in base alla configurazione dell'indirizzo IP, subnet mask e gateway predefinito per ogni interfaccia LAN. Determinazione automatica della metrica interfaccia, abilitata per impostazione predefinita, determina la velocità di ogni interfaccia e modifica le metriche delle route per ogni interfaccia in modo che l'interfaccia più veloce consente di creare le route con il valore metrico più basso. Per rimuovere le metriche di grandi dimensioni, disattivare il rilevamento automatico della metrica interfaccia dalle proprietà avanzate del protocollo TCP/IP per ogni connessione LAN.  
-   Nomi possono essere usati per *destinazione* se esiste una voce adatta nel file di reti locale archiviato nel **systemroot\System32\Drivers\\**cartella e così via. Nomi possono essere usati per la *gateway* purché può essere risolte in un indirizzo IP tramite le tecniche di risoluzione nome host standard, ad esempio le query di sistema DNS (Domain Name), usare il file Hosts locale archiviato nel  **systemroot\system32\drivers\\**cartella e così via e la risoluzione dei nomi NetBIOS.  
-   Se il comando **stampare** oppure **eliminare**, il *Gateway* parametro può essere omesso e i caratteri jolly può essere utilizzato per la destinazione e il gateway. Il *destinazione* valore può essere un valore carattere jolly specificato da un asterisco (*). Se la destinazione specificata contiene un asterisco (\*) o un punto interrogativo (?), viene considerato come un carattere jolly e solo le route corrispondenti vengono stampate o eliminate. L'asterisco corrisponde a qualsiasi stringa e il punto interrogativo corrisponde a qualsiasi carattere singolo. Ad esempio, 10. \*.1, 192.168. \*, 127. \*, e \*224\* sono tutte validi usi del carattere jolly asterisco.  
-   Esegue una combinazione non valida di un valore di destinazione e la subnet mask (subnet mask), verrà visualizzata una "Route: indirizzo gateway non valido" messaggio di errore. Questo messaggio di errore viene visualizzato quando la destinazione contiene uno o più bit impostati su 1 in posizioni di bit in cui il bit di maschera di subnet corrispondente è impostato su 0. Per testare questa condizione, esprimere la destinazione e una subnet mask usando la notazione binaria. La subnet mask nella notazione binaria è costituito da una serie di bit 1, che rappresenta la parte di indirizzo di rete di destinazione e una serie di bit 0, che rappresenta la parte relativa all'indirizzo host dell'oggetto di destinazione. Selezionare questa opzione per determinare se sono presenti bit nella destinazione che sono impostati su 1 per la parte di destinazione che rappresenta l'indirizzo dell'host (come definito dalla subnet mask).  
-   Il **/p** parametro è supportato solo per il comando di route per Windows NT 4.0, Windows 2000, Windows Millennium edition, Windows XP e Windows Server 2003. Questo parametro non è supportato per il **route** comando per Windows 95 o Windows 98.  
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.  

## <a name="BKMK_Examples"></a>Esempi  
Per visualizzare l'intero contenuto della tabella di routing IP, digitare:  
```  
route print  
```  
Per visualizzare le route nella tabella di routing IP che iniziano con 10, digitare:  
```  
route print 10.*  
```  
Per aggiungere una route predefinita con l'indirizzo gateway predefinito di 192.168.12.1, digitare:  
```  
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1  
```  
Per aggiungere una route per la destinazione 10.41.0.0 con la subnet mask 255.255.0.0 e l'indirizzo hop successivo di 10.27.0.1, digitare:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Per aggiungere una route persistente nella destinazione 10.41.0.0 con la subnet mask 255.255.0.0 e l'indirizzo hop successivo di 10.27.0.1, digitare:  
```  
route /p add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Per aggiungere una route per la destinazione 10.41.0.0 con la subnet mask 255.255.0.0, l'indirizzo hop successivo di 10.27.0.1 e la metrica di costo pari a 7, digitare:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 metric 7  
```  
Per aggiungere una route per la destinazione 10.41.0.0 con la subnet mask 255.255.0.0, l'indirizzo hop successivo di 10.27.0.1 e utilizzare l'indice dell'interfaccia 0x3, digitare:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 if 0x3  
```  
Per eliminare la route per la destinazione 10.41.0.0 con la subnet mask 255.255.0.0, digitare:  
```  
route delete 10.41.0.0 mask 255.255.0.0  
```  
Per eliminare tutte le route nella tabella di routing IP che iniziano con 10, digitare:  
```  
route delete 10.*  
```  
Per modificare l'indirizzo hop successivo della route con destinazione 10.41.0.0 e la subnet mask 255.255.0.0 da 10.27.0.1 a 10.27.0.25, digitare:  
```  
route change 10.41.0.0 mask 255.255.0.0 10.27.0.25  
```  

## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
