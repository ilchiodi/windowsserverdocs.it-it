---
title: Novità di DHCP
description: In questo argomento viene fornita una panoramica delle nuove funzionalità per Dynamic Host Configuration Protocol (DHCP) in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73cc5134f7af5063c912ad578fa7d660b3194aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840232"
---
# <a name="whats-new-in-dhcp"></a>Novità di DHCP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono descritte le funzionalità di Dynamic Host Configuration Protocol (DHCP) nuove o modificate in Windows Server 2016.
  
DHCP è uno standard di Internet Engineering Task Force (IETF) che è progettato per ridurre il carico amministrativo e la complessità della configurazione degli host in un TCP/IP\-basati su rete, ad esempio una intranet privata. Se si usa il servizio server DHCP, il processo di configurazione di TCP/IP nei client DHCP è automatico.

Le sezioni seguenti forniscono informazioni sulle nuove funzionalità e modifiche delle funzionalità di DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opzioni di selezione Subnet DHCP

DHCP supporta ora opzioni 118 e 82 \(Sub-opzione 5\). È possibile usare queste opzioni per consentire i client proxy DHCP e gli agenti di inoltro richiedere un indirizzo IP per una subnet specifica e da un intervallo di indirizzi IP specifico e un ambito.


Se si usa un agente di inoltro DHCP configurato con l'opzione DHCP 82, sub\-opzione 5, l'agente di inoltro può richiedere un lease di indirizzi IP per i client DHCP da un intervallo di indirizzi IP specifico.

Per altre informazioni, vedere [opzioni di selezione Subnet DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nuova registrazione di eventi per gli errori di registrazione DNS dal Server DHCP

DHCP include ora la registrazione eventi nelle circostanze in cui DHCP record registrazioni dei server DNS non nel server DNS.

Per altre informazioni, vedere [DHCP eventi di registrazione per le registrazioni dei Record DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Protezione accesso alla rete DHCP non è supportato in Windows Server 2016

Protezione accesso alla rete \(NAP\) è deprecato in Windows Server 2012 R2 e in Windows Server 2016 il ruolo Server DHCP non supporta Protezione accesso alla rete. Per ulteriori informazioni, vedere [funzionalità rimosse o deprecate in Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
Supporto di NAP è stata introdotta per il ruolo Server DHCP con Windows Server 2008 ed è supportato nei sistemi di operativo di client e server Windows precedenti a Windows 10 e Windows Server 2016. Nella tabella seguente viene riepilogato il supporto per protezione accesso alla RETE in Windows Server.  
  
|Sistema operativo|Supporto di NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Supportato|  
| Windows Server 2008 R2 |Supportato|  
| Windows Server 2012 |Supportato|  
| Windows Server 2012 R2 |Supportato|  
| Windows Server 2016|Non supportato|  
  
In una distribuzione di protezione accesso alla RETE, server DHCP in esecuzione un sistema operativo che supporta Protezione accesso alla RETE possono essere utilizzati come punto di imposizione Protezione accesso alla RETE per il metodo di imposizione Protezione accesso alla RETE DHCP. Per altre informazioni su Protezione accesso alla rete DHCP, vedere [elenco di controllo: Implementazione di una progettazione di imposizione DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
In Windows Server 2016, i server DHCP non applicano i criteri di protezione accesso alla rete e gli ambiti DHCP non possono essere NAP\-abilitata. I computer client DHCP che sono anche i client Protezione accesso alla rete inviano un rapporto di integrità \(rapporto di integrità\) con la richiesta DHCP. Se il server DHCP è in esecuzione Windows Server 2016, queste richieste vengono elaborate come se non è presente alcun rapporto di integrità. Il server DHCP concede un lease DHCP normale al client. 

Se i server che eseguono Windows Server 2016 proxy RADIUS che inoltra le richieste di autenticazione a un Server criteri di rete \(NPS\) che supporta Protezione accesso alla rete, questi client Protezione accesso alla rete vengono valutati tramite criteri di rete come protezione accesso alla rete non\-in grado di supportare, e Protezione accesso alla rete di elaborazione ha esito negativo.
  
## <a name="see-also"></a>Vedere anche  
  
-   [Dynamic Host Configuration Protocol (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

