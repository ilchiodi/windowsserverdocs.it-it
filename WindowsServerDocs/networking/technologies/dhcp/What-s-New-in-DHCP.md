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
ms.openlocfilehash: f67e5dfd8aefcf408a41f6fc919665419f0ff1c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dhcp"></a>Novità di DHCP

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento descrive le funzionalità di Dynamic Host Configuration Protocol (DHCP) nuove o modificate in Windows Server 2016.
  
DHCP è uno standard di Internet Engineering Task Force (IETF) che è progettato per ridurre il carico amministrativo e la complessità della configurazione degli host in una rete basata su TCP/IP\, ad esempio una intranet privata. Utilizzando il servizio Server DHCP, il processo di configurazione TCP/IP nei client DHCP è automatico.

Le sezioni seguenti forniscono informazioni sulle nuove funzionalità e modifiche delle funzionalità di DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opzioni di selezione Subnet DHCP

DHCP supporta ora \(sub-option 5\) opzioni 118 e 82. È possibile utilizzare queste opzioni per consentire ai client proxy DHCP e gli agenti di inoltro richiedere un indirizzo IP per una subnet specifica e da un intervallo di indirizzi IP specifico e l'ambito.

Se si utilizza un client proxy DHCP configurato con l'opzione DHCP 118, ad esempio un server \(VPN\) rete privata virtuale che esegue Windows Server 2016 e il ruolo server di accesso remoto, il server VPN può richiedere un lease di indirizzi IP per i client VPN da un intervallo di indirizzi IP specifico.

Se si utilizza un agente di inoltro DHCP configurato con l'opzione DHCP 82, sub\-opzione 5, l'agente di inoltro può richiedere un lease di indirizzi IP per i client DHCP da un intervallo di indirizzi IP specifico.

Per ulteriori informazioni, vedere [opzioni di selezione Subnet DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nuovi eventi di registrazione per gli errori di registrazione DNS dal Server DHCP

DHCP include ora la registrazione eventi per i casi in cui DHCP esito negativo registrazioni record DNS del server nel server DNS.

Per ulteriori informazioni, vedere [DHCP eventi di registrazione per le registrazioni dei Record DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Protezione accesso alla rete DHCP non è supportato in Windows Server 2016

\(NAP\) protezione accesso alla rete è deprecato in Windows Server 2012 R2 e in Windows Server 2016 il ruolo Server DHCP non supporta Protezione accesso alla rete. Per ulteriori informazioni, vedere [funzionalità rimosse o deprecate in Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
Il supporto di NAP è stata introdotta per il ruolo Server DHCP con Windows Server 2008 ed è supportato nel client e server di sistemi operativi precedenti a Windows 10 e Windows Server 2016. Nella tabella seguente viene riepilogato il supporto per protezione accesso alla rete in Windows Server.  
  
|Sistema operativo|Supporto di NAP|  
|--------------------|---------------|  
| Windows Server 2008 |È supportato|  
| Windows Server 2008 R2 |È supportato|  
| Windows Server 2012 |È supportato|  
| Windows Server 2012 R2 |È supportato|  
| Windows Server 2016|Non è supportato|  
  
In una distribuzione di protezione accesso alla rete, server DHCP che eseguono un sistema operativo che supporta Protezione accesso alla rete possono essere utilizzati come punto di imposizione Protezione accesso alla rete per il metodo di imposizione Protezione accesso alla rete DHCP. Per ulteriori informazioni su Protezione accesso alla rete DHCP, vedere [elenco di controllo: implementazione di una progettazione di imposizione DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
In Windows Server 2016, server DHCP non applicano i criteri di protezione accesso alla rete e gli ambiti DHCP non possono essere abilitato NAP\. I computer client DHCP che sono anche i client di protezione accesso alla rete inviano un rapporto di integrità \(SoH\) con la richiesta DHCP. Se il server DHCP è in esecuzione Windows Server 2016, tali richieste vengono elaborate come se non è presente nessun rapporto di integrità. Il server DHCP concede a un lease DHCP normale al client. 

Se i server che eseguono Windows Server 2016 proxy RADIUS che inoltra le richieste di autenticazione per un \(NPS\) Server dei criteri di rete che supporta Protezione accesso alla rete, questi client Protezione accesso alla rete vengono valutati come non NAP\ in grado di supportare e protezione accesso alla rete si verifica un errore di elaborazione dei criteri di rete.
  
## <a name="see-also"></a>Vedere anche  
  
-   [Dynamic Host Configuration Protocol (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

