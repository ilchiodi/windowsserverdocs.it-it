---
title: Utilizzando il comando /New-multicasttransmission
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84f5aa69f2d4a875995ac6c18fa43bd68518fba3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875142"
---
# <a name="using-the-new-multicasttransmission-command"></a>Utilizzando il comando /New-multicasttransmission

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una nuova trasmissione multicast per un'immagine. Questo comando è equivalente alla creazione di una trasmissione tramite lo snap-in mmc servizi di distribuzione Windows (pulsante destro del mouse il **trasmissioni Multicast** nodo e quindi fare clic su **creare trasmissione Multicast**). Utilizzare questo comando quando si dispone di Server di trasporto installato il servizio ruolo, ovvero l'installazione predefinita, e il servizio ruolo Server di distribuzione. Se si dispone solo Server di trasporto installato il servizio ruolo, utilizzare [utilizzando il comando nuovo spazio dei nomi](using-the-new-namespace-command.md).
## <a name="syntax"></a>Sintassi
per le trasmissioni di immagini di installazione:
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
per le trasmissioni di immagini d'avvio (supportate solo per Windows Server 2008 R2):
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media:<Image name>|Specifica il nome dell'immagine devono essere trasmessi tramite il multicast.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/FriendlyName:<Friendly name>|Specifica il nome descrittivo della trasmissione.|
|[/ Descrizione:<Description>]|Specifica la descrizione della trasmissione.|
mediatype:{Boot&#124;Install}|Specifica il tipo di immagine devono essere trasmessi tramite il multicast. Nota **avvio** è supportato solo per Windows Server 2008 R2.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini che contiene l'immagine. Se viene specificato alcun nome di gruppo di immagini e il gruppo solo un'immagine esistente sul server, viene utilizzato il gruppo di immagini. Se è presente più di un gruppo di immagini nel server, è necessario utilizzare questa opzione per specificare il nome del gruppo di immagini.|
|[/Filename:<File name>]|Specifica il nome del file. Se l'immagine di origine non può essere identificata in modo univoco dal nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|/Transmissiontype:{AutoCast &#124; ScheduledCast}|Specifica se avviare automaticamente la trasmissione (automatico) o in base ai criteri di inizio specificato (multicast).<br /><br /><ul><li>**Multicast automatico**. Questo tipo di trasmissione indica che non appena un client richiede un'immagine di installazione, ha inizio la trasmissione multicast dell'immagine selezionata. Se altri client richiedono la stessa immagine, in cui vengono aggiunti la trasmissione che è già stata avviata.</li><li>**Il multicast pianificato**. Questo tipo di trasmissione imposta i criteri per la trasmissione in base al numero di client che richiedono un'immagine e/o un giorno specifico e l'ora di avvio. È possibile specificare le opzioni seguenti:<br /><br /><ul><li>[/ ora: <time>]-imposta il tempo di trasmissione deve iniziare con il formato seguente: YYYY/MM/DD:hh:mm.</li><li>[/ Client: <Number of clients>] -Imposta il numero minimo di client da attendere prima che venga avviata la trasmissione.</li></ul></li></ul>|
|/ Architettura: {x86 & #124; ia64 & #124; x64}|Specifica l'architettura dell'immagine di avvio per la trasmissione mediante il multicast. Poiché è possibile avere lo stesso nome per le immagini di avvio di diverse architetture, è necessario specificare l'architettura per garantire che venga utilizzata l'immagine corretta.|
|[/Filename:<File name>]|Specifica il nome del file. Se l'immagine di origine non può essere identificata in modo univoco dal nome, è necessario specificare il nome del file.|
## <a name="BKMK_examples"></a>Esempi
Per creare una trasmissione multicast automatico di un'immagine di avvio in Windows Server 2008 R2, digitare:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS Boot Transmission"
/Image:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
Per creare una trasmissione multicast automatico di un'immagine di installazione, digitare:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS AutoCast Transmission"
/Image:"Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
Per creare una trasmissione multicast pianificato di un'immagine di installazione, digitare:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS SchedCast Transmission" /Server:MyWDSServemedia:"Vista with Officemediatype:Install 
/Transmissiontype:ScheduledCast /time:"2006/11/20:17:00" /Clients:100
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[utilizzando il comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
[utilizzando il comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[sottocomando: inizio MulticastTransmission](subcommand-start-multicasttransmission.md)
