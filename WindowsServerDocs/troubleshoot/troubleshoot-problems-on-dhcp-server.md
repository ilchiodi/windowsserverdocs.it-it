---
title: Risolvere i problemi nel server DHCP
description: Questo artilce illustra come risolvere i problemi nel server DHCP e raccogliere i dati.
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: ad70b03fcb6d703a0b99435ee8715319d09af941
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150199"
---
# <a name="troubleshoot-problems-on-the-dhcp-server"></a>Risolvere i problemi nel server DHCP

Questo articolo illustra come risolvere i problemi che si verificano nel server DHCP.

## <a name="troubleshooting-checklist"></a>Elenco di controllo per la risoluzione dei problemi

Controllare le impostazioni seguenti:

  - Il servizio server DHCP è avviato e in esecuzione. Per controllare questa impostazione, eseguire il comando **net start** e cercare **server DHCP**.

  - Il server DHCP è autorizzato. Vedere [autorizzazione server DHCP Windows in scenario aggiunto a un dominio](https://docs.microsoft.com/openspecs/windows_protocols/ms-dhcpe/56f8870b-a7c1-4db1-8a86-f69079fe5077).

  - Verificare che i lease di indirizzi IP siano disponibili nell'ambito del server DHCP per la subnet in cui si trova il client DHCP. A tale scopo, vedere le statistiche relative all'ambito appropriato nella console di gestione server DHCP.

  - Controllare se \_ sono presenti elenchi di indirizzi non validi nei lease di indirizzi.

  - Controllare se nei dispositivi della rete sono presenti indirizzi IP statici che non sono stati esclusi dall'ambito DHCP.

  - Verificare che l'indirizzo IP a cui è associato il server DHCP si trovi all'interno della subnet degli ambiti da cui è necessario eseguire il lease degli indirizzi IP. Questa situazione si verifica nel caso in cui non sia disponibile alcun agente di inoltro. A tale scopo, eseguire il cmdlet **Get-DhcpServerv4Binding** o **Get-DhcpServerv6Binding** .

  - Verificare che solo il server DHCP sia in ascolto sulla porta UDP 67 e 68. Nessun altro processo o altro servizio (ad esempio WDS o PXE) deve occupare queste porte. A tale scopo, eseguire il `netstat -anb` comando.

  - Verificare che l'esenzione IPsec del server DHCP venga aggiunta se si sta utilizzando un ambiente distribuito tramite IPsec.

  - Verificare che sia possibile eseguire il ping dell'indirizzo IP dell'agente di inoltro dal server DHCP.

  - Enumerare e controllare i criteri e i filtri DHCP configurati.

## <a name="event-logs"></a>Log eventi

Controllare i registri eventi del servizio server di sistema e DHCP (**registri applicazioni e servizi** \> **Microsoft** \> **Windows** \> **DHCP-server**) per i problemi segnalati correlati al problema riscontrato.  
A seconda del tipo di problema, viene registrato un evento in uno dei canali di eventi seguenti:  
[Eventi operativi server DHCP](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))  
[Eventi amministrativi del server DHCP](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))  
[Eventi di sistema del server DHCP](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))  
[Eventi di notifica filtro server DHCP](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))  
[Eventi di controllo del server DHCP](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))

## <a name="data-collection"></a>Raccolta dati

### <a name="dhcp-server-log"></a>Log del server DHCP

I log di debug del servizio server DHCP forniscono ulteriori informazioni sull'assegnazione di lease degli indirizzi IP e sugli aggiornamenti dinamici DNS eseguiti dal server DHCP. Per impostazione predefinita, questi log si trovano in% windir% \\ system32 \\ DHCP.  
Per ulteriori informazioni, vedere [analizzare i file di log del server DHCP](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd183591\(v=ws.10\)).

### <a name="network-trace"></a>Traccia di rete

Una traccia di rete correlata può indicare il comportamento del server DHCP nel momento in cui l'evento è stato registrato. Per creare una traccia di questo tipo, attenersi alla seguente procedura:

1.  Passare a [GitHub](https://github.com/CSS-Windows/WindowsDiag/tree/master/ALL/TSS)e scaricare il file con estensione [ \_ zip degli strumenti di TSS](https://github.com/CSS-Windows/WindowsDiag/blob/master/ALL/TSS/tss_tools.zip) .

2.  Copiare il \_ file con estensione zip degli strumenti TSS ed espanderlo in un percorso sul disco locale, ad esempio nella cartella C: \\ Tools.

3.  Eseguire il comando seguente da C: \\ Tools in una finestra del prompt dei comandi con privilegi elevati:  
    ```console
    TSS Ron Trace <Stop:Evt:>20321:<Other:>DhcpAdminEvents NoSDP NoPSR NoProcmon NoGPresult
    ```
      
    >[!Note]
    >In questo comando sostituire \<*Stop:Evt:*\> e \<*Other:*\> con l'ID evento e il canale di eventi su cui si intende concentrarsi nella sessione di traccia.  
    >I \_ \_ file con estensione. docx della Guida di TSS. cmd contenuti nel \_ file con estensione zip di strumenti TSS forniscono ulteriori informazioni su tutte le impostazioni disponibili.

4.  Dopo l'attivazione dell'evento, lo strumento crea una cartella denominata C: \\ MS \_ Data. Questa cartella conterrà alcuni file di output utili che forniscono informazioni generali sulla configurazione di rete e dominio del computer.  
    Il file più interessante in questa cartella è% ComputerName% \_ data/ \_ ora \_ packetcapture \_ internetClient \_ dbg. etl.
    Utilizzando l'applicazione [Network Monitor](https://www.microsoft.com/download/4865) , è possibile caricare il file e impostare il filtro di visualizzazione sul protocollo "DHCP o DNS" per esaminare le operazioni eseguite dietro le quinte.
