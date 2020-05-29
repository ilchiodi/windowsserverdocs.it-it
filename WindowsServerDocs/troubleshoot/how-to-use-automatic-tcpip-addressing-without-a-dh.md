---
title: Come utilizzare indirizzi TCP/IP automatici senza un server DHCP
description: Introduzione all'utilizzo di indirizzi TCP/IP automatici senza un server DHCP.
ms.date: 5/26/2020
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.reviewer: robsmi
ms.openlocfilehash: 8fbde77381141b76959f70e824eac22ee2121fa3
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150189"
---
# <a name="how-to-use-automatic-tcpip-addressing-without-a-dhcp-server"></a>Come utilizzare indirizzi TCP/IP automatici senza un server DHCP

Questo articolo descrive come usare l'indirizzamento automatico Transmission Control Protocol/Internet Protocol (TCP/IP) senza che sia presente un server di Dynamic Host Configuration Protocol (DHCP) in rete. Le versioni del sistema operativo elencate nella sezione "si applica a" di questo articolo hanno una funzionalità denominata indirizzo IP privato automatico (APIPA, Automatic Private IP Addressing). Con questa funzionalità, un computer Windows può assegnare un indirizzo IP (Internet Protocol) nel caso in cui un server DHCP non sia disponibile o non esista nella rete. Questa funzionalità consente di configurare e supportare una piccola rete locale (LAN) che esegue TCP/IP meno difficile.

## <a name="more-information"></a>Altre informazioni

> [!IMPORTANT]  
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/help/322756) nel caso in cui si verifichino problemi.

Un computer basato su Windows configurato per l'utilizzo di DHCP può assegnare automaticamente un indirizzo IP (Internet Protocol) se non è disponibile un server DHCP. Questo può verificarsi, ad esempio, in una rete senza un server DHCP o in una rete se un server DHCP è temporaneamente inattivo per la manutenzione.

IANA (Internet Assigned Numbers Authority) ha riservato 169.254.0.0-169.254.255.255 per gli indirizzi IP privati automatici. Di conseguenza, APIPA fornisce un indirizzo che non è in conflitto con gli indirizzi instradabili.

Dopo che alla scheda di rete è stato assegnato un indirizzo IP, il computer può utilizzare TCP/IP per comunicare con qualsiasi altro computer connesso alla stessa LAN e che è anche configurato per APIPA oppure l'indirizzo IP è impostato manualmente su 169.254. x. y (dove x. y è l'identificatore univoco del client) con un subnet mask di 255.255.0.0. Si noti che il computer non è in grado di comunicare con i computer in altre subnet o con computer che non utilizzano indirizzi IP privati automatici. Gli indirizzi IP privati automatici sono abilitati per impostazione predefinita.

Potrebbe essere necessario disabilitarlo nei casi seguenti:

- La rete usa i router.

- La rete è connessa a Internet senza un server NAT o proxy.

A meno che non siano stati disabilitati i messaggi relativi a DHCP, i messaggi DHCP forniscono notifiche quando si passa da un indirizzo DHCP a un indirizzo IP privato automatico e viceversa. Se la messaggistica DHCP viene accidentalmente disabilitata, è possibile riattivare i messaggi DHCP modificando il valore del valore PopupFlag nella seguente chiave del registro di sistema da 00 a 01:  
`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP` 

Si noti che è necessario riavviare il computer per rendere effettive le modifiche. È inoltre possibile determinare se il computer utilizza APIPA utilizzando lo strumento Winipcfg in Windows Millennium Edition, Windows 98 o Windows 98 Second Edition:

Fare clic sul pulsante Start, scegliere Esegui, digitare "winipcfg" (senza le virgolette), quindi fare clic su OK. Fare clic su altre informazioni. Se la casella Indirizzo configurazione automatica IP contiene un indirizzo IP compreso nell'intervallo 169.254. x. x, l'indirizzamento IP privato automatico è abilitato. Se la casella indirizzo IP esiste, gli indirizzi IP privati automatici non sono attualmente abilitati.
Per Windows 2000, Windows XP o Windows Server 2003, è possibile determinare se il computer utilizza APIPA utilizzando il comando IPconfig a un prompt dei comandi:

Fare clic sul pulsante Start, scegliere Esegui, digitare "cmd" (senza le virgolette), quindi fare clic su OK per aprire una finestra della riga di comando MS-DOS. Digitare "ipconfig/" (senza virgolette), quindi premere il tasto INVIO. Se la riga ' Autoconfiguration Enabled ' indica "Yes" è Autoconfiguration IP Address ' è 169.254. x. y (dove x. y è l'identificatore univoco del client), il computer usa APIPA. Se la riga ' Autoconfiguration Enabled ' indica "No", il computer non sta attualmente utilizzando APIPA.
È possibile disabilitare gli indirizzi IP privati automatici usando uno dei metodi seguenti.

È possibile configurare le informazioni TCP/IP manualmente, in modo da disabilitare completamente DHCP. È possibile disabilitare gli indirizzi IP privati automatici (ma non DHCP) modificando il registro di sistema. A tale scopo, aggiungere la voce del registro di sistema DWORD "IPAutoconfigurationEnabled" con il valore 0x0 alla chiave del registro di sistema seguente per Windows Millennium Edition, Windows98 o Windows 98 Second Edition:`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP`

Per Windows 2000, Windows XP e Windows Server 2003, è possibile disabilitare APIPA aggiungendo la voce del registro di sistema DWORD "IPAutoconfigurationEnabled" con il valore 0x0 alla chiave del registro di sistema seguente:  
`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\<Adapter GUID>`  
> [!NOTE]
> La sottochiave **GUID dell'adapter** è un identificatore univoco globale (Guid) per la scheda LAN del computer.

Se si specifica un valore pari a 1 per la voce DWORD IPAutoconfigurationEnabled, verrà abilitato APIPA, che è lo stato predefinito quando questo valore viene omesso dal registro di sistema.

## <a name="examples-of-where-apipa-may-be-useful"></a>Esempi di dove APIPA può essere utile

### <a name="example-1-no-previous-ip-address-and-no-dhcp-server"></a>Esempio 1: nessun indirizzo IP precedente e nessun server DHCP

Quando il computer basato su Windows (configurato per DHCP) viene inizializzato, trasmette tre o più messaggi di "individuazione". Se un server DHCP non risponde dopo la trasmissione di diversi messaggi di individuazione, il computer Windows assegna a se stesso un indirizzo di classe B (APIPA). Il computer Windows visualizzerà quindi un messaggio di errore all'utente del computer (a condizione che non sia mai stato assegnato un indirizzo IP da un server DHCP in passato). Il computer Windows invierà quindi un messaggio di individuazione ogni tre minuti nel tentativo di stabilire le comunicazioni con un server DHCP.

### <a name="example-2-previous-ip-address-and-no-dhcp-server"></a>Esempio 2: indirizzo IP precedente e nessun server DHCP

Il computer controlla il server DHCP e, se non ne viene trovato alcuno, viene effettuato un tentativo di contattare il gateway predefinito. Se il gateway predefinito risponde, il computer Windows mantiene l'indirizzo IP precedentemente concesso in lease. Tuttavia, se il computer non riceve una risposta dal gateway predefinito o se non ne viene assegnato alcuno, usa la funzionalità di indirizzamento IP privato automatico per assegnare se stessa un indirizzo IP. Viene visualizzato un messaggio di errore all'utente e i messaggi di individuazione vengono trasmessi ogni 3 minuti. Una volta che un server DHCP è in linea, viene generato un messaggio che informa che le comunicazioni sono state ristabilite con un server DHCP.

### <a name="example-3-lease-expires-and-no-dhcp-server"></a>Esempio 3: scadenza lease e nessun server DHCP

Il computer basato su Windows tenta di ristabilire il lease dell'indirizzo IP. Se il computer Windows non trova un server DCHP, viene assegnato un indirizzo IP dopo la generazione di un messaggio di errore. Il computer trasmette quindi quattro messaggi di individuazione e, dopo ogni 5 minuti, ripete l'intera procedura fino a quando non viene visualizzato un server DHCP. Viene quindi generato un messaggio che informa che le comunicazioni sono state ristabilite con il server DHCP.
