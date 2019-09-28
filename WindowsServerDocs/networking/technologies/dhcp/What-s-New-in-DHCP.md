---
title: Novità di DHCP
description: Questo argomento fornisce una panoramica delle nuove funzionalità per Dynamic Host Configuration Protocol (DHCP) in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8032b7c8e78170d57b0367775672577d9fd900e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355445"
---
# <a name="whats-new-in-dhcp"></a>Novità di DHCP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono descritte le funzionalità di Dynamic Host Configuration Protocol (DHCP) nuove o modificate in Windows Server 2016.
  
DHCP è uno standard IETF (Internet Engineering Task Force) progettato per ridurre il carico amministrativo e la complessità della configurazione degli host in una rete TCP/IP @ no__t-0based, ad esempio una Intranet privata. Se si usa il servizio server DHCP, il processo di configurazione di TCP/IP nei client DHCP è automatico.

Nelle sezioni seguenti vengono fornite informazioni sulle nuove funzionalità e sulle modifiche apportate alle funzionalità per DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opzioni di selezione subnet DHCP

DHCP supporta ora le opzioni 118 e 82 \(Sub-Option 5 @ no__t-1. È possibile utilizzare queste opzioni per consentire ai client proxy DHCP e agli agenti di inoltro di richiedere un indirizzo IP per una subnet specifica e da un intervallo di indirizzi IP e un ambito specifici.


Se si usa un agente di inoltro DHCP configurato con l'opzione DHCP 82, Sub @ no__t-0option 5, l'agente di inoltro può richiedere un lease di indirizzi IP per i client DHCP da un intervallo di indirizzi IP specifico.

Per ulteriori informazioni, vedere [Opzioni di selezione della subnet DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nuovi eventi di registrazione per errori di registrazione DNS da parte del server DHCP

DHCP include ora eventi di registrazione per le circostanze in cui le registrazioni dei record DNS del server DHCP hanno esito negativo nel server DNS.

Per altre informazioni, vedere [eventi di registrazione DHCP per registrazioni di record DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Protezione accesso alla rete DHCP non è supportata in Windows Server 2016

Protezione accesso alla rete \(NAP @ no__t-1 è deprecato in Windows Server 2012 R2 e in Windows Server 2016 il ruolo server DHCP non supporta più NAP. Per ulteriori informazioni, vedere [funzionalità rimosse o deprecate in Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
Il supporto di protezione accesso alla rete è stato introdotto per il ruolo server DHCP con Windows Server 2008 ed è supportato nei sistemi operativi client e server Windows precedenti a Windows 10 e Windows Server 2016. Nella tabella seguente viene riepilogato il supporto per protezione accesso alla RETE in Windows Server.  
  
|Sistema operativo|Supporto di NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Supportato|  
| Windows Server 2008 R2 |Supportato|  
| Windows Server 2012 |Supportato|  
| Windows Server 2012 R2 |Supportato|  
| Windows Server 2016|Non supportate|  
  
In una distribuzione di protezione accesso alla RETE, server DHCP in esecuzione un sistema operativo che supporta Protezione accesso alla RETE possono essere utilizzati come punto di imposizione Protezione accesso alla RETE per il metodo di imposizione Protezione accesso alla RETE DHCP. Per ulteriori informazioni su DHCP in NAP, vedere [Checklist: Implementazione di una progettazione di imposizione DHCP @ no__t-0.  
  
In Windows Server 2016, i server DHCP non applicano i criteri di protezione accesso alla rete e gli ambiti DHCP non possono essere NAP @ no__t-0enabled. I computer client DHCP che sono anche client di protezione accesso alla rete inviano un rapporto di integrità \(SoH @ no__t-1 con la richiesta DHCP. Se nel server DHCP è in esecuzione Windows Server 2016, le richieste vengono elaborate come se non fosse presente il rapporto di integrità. Il server DHCP concede al client un normale lease DHCP. 

Se i server che eseguono Windows Server 2016 sono proxy RADIUS che inoltrano richieste di autenticazione a un server dei criteri di rete \(NPS @ no__t-1 che supporta NAP, questi client di protezione accesso alla rete vengono valutati da NPS come non NAP @ no__t-2capable e l'elaborazione NAP non riesce.
  
## <a name="see-also"></a>Vedere anche  
  
-   [Dynamic Host Configuration Protocol (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

