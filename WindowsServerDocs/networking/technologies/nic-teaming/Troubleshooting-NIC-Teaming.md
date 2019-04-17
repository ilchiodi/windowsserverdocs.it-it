---
title: Risoluzione dei problemi di gruppo NIC
description: In questo argomento fornisce informazioni sulla risoluzione dei problemi di gruppo NIC in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 634ec1a5eee0cd661ca89c2673e5b74abd458cd6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-nic-teaming"></a>Risoluzione dei problemi di gruppo NIC

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento vengono fornite informazioni sulla risoluzione dei problemi di gruppo NIC e contiene le sezioni seguenti, descritte le possibili cause dei problemi con gruppo NIC.  
  
-   [Hardware che non sono conformi alla specifica](#bkmk_hardware)  
  
-   [Funzionalità di sicurezza commutatore fisico](#bkmk_switch)  
  
-   [Disabilitazione e abilitazione con Windows PowerShell](#bkmk_ps)  
  
## <a name="bkmk_hardware"></a>Hardware che non sono conformi alla specifica  
Quando implementazioni hardware di protocolli standard non sono conformi alla specifica, gruppo NIC prestazioni potrebbero risentirne.  
  
Durante il normale funzionamento, gruppo NIC può inviare i pacchetti dello stesso indirizzo IP, ma con più accesso ai file multimediali di origine diversi indirizzi control (MAC). In base a standard di protocolli, i destinatari di questi pacchetti devono risolvere l'indirizzo IP dell'host o VM a un indirizzo MAC specifico anziché risponde all'indirizzo MAC da cui è stato ricevuto il pacchetto.  I client che implementano correttamente i protocolli di risoluzione, indirizzo protocollo ARP del IPv4 (Address Resolution) o router adiacente discovery protocol del IPv6 (PIN), verranno inviati i pacchetti con l'indirizzo MAC (l'indirizzo MAC della macchina virtuale o host a cui appartiene l'indirizzo IP) di destinazione corretto.  
  
Alcuni incorporato hardware, tuttavia, non implementa i protocolli di risoluzione indirizzi correttamente e anche potrebbe non esplicitamente risolvere un indirizzo IP a un indirizzo MAC utilizzando ARP o PIN.  Un controller di rete (SAN) area di archiviazione è un esempio di un dispositivo che potrebbe eseguire in questo modo. I dispositivi non conformi copiare l'indirizzo MAC contenuto di origine in un pacchetto ricevuto e utilizzano per l'indirizzo MAC nei pacchetti in uscita corrispondenti di destinazione.  
  
Di conseguenza i pacchetti inviati per l'indirizzo MAC di destinazione errato. Per questo motivo, i pacchetti vengono eliminati dal commutatore virtuale Hyper-V perché non corrispondono qualsiasi destinazione noti.  
  
Se si è visto problemi con la connessione al controller di rete SAN o altro incorporato hardware, deve richiedere acquisizioni di pacchetti e determinare se l'hardware viene implementata correttamente ARP o PIN e contattare il fornitore dell'hardware per ottenere assistenza.  
  
## <a name="bkmk_switch"></a>Funzionalità di sicurezza commutatore fisico  
A seconda della configurazione, gruppo NIC può inviare i pacchetti dello stesso indirizzo IP con più indirizzi MAC di origine diversa.  Questo può attivarsi la protezione, evitare di funzionalità del commutatore fisico, ad esempio ispezione ARP dinamico o IP origine, in particolare se il commutatore fisico non è compatibile con che le porte fanno parte di un gruppo. Ciò può verificarsi se configurare gruppo NIC in modalità indipendenti commutatore.  È consigliabile esaminare i registri del commutatore per determinare se la funzionalità di sicurezza commutatore causano problemi di connettività con gruppo NIC.  
  
## <a name="bkmk_ps"></a>Disabilitazione e abilitazione di schede di rete utilizzando Windows PowerShell  
Un motivo comune per un gruppo NIC avere esito negativo è che l'interfaccia del team è disabilitato. In molti casi, l'interfaccia è disattivata accidentalmente quando viene eseguita la sequenza di comandi Windows PowerShell seguente:  
  
```  
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  
Questa sequenza di comandi non abilita tutti NetAdapters che è disabilitato.  
  
Questo avviene perché se si disattiva tutte le schede NIC membro fisico sottostante, l'interfaccia del team NIC da rimuovere e non sono più visualizzati nella Get-NetAdapter. Per questo motivo, il **Enable-NetAdapter \ *** comando non attiva il gruppo NIC, dal momento che la scheda viene rimosso.  
  
Il **Enable-NetAdapter \ *** comando, tuttavia, abilitare il membro schede di rete, che quindi (dopo un breve periodo di tempo) fa sì che l'interfaccia del team verrà ricreato. In questo caso, l'interfaccia del team è ancora in uno stato "disabled" poiché non è stato riabilitato. Abilitare l'interfaccia del team dopo che viene ricreato consentirà il traffico di rete iniziare a flusso nuovamente.  
  
## <a name="see-also"></a>Vedere anche  
[Gruppo NIC](NIC-Teaming.md)  
  


