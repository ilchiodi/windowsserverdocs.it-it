---
title: New-MulticastTransmission
description: Argomento di riferimento per New-MulticastTransmission, che consente di creare una nuova trasmissione multicast per un'immagine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 779dd0937f6889f795268bf59aa35837e84aca42
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720883"
---
# <a name="new-multicasttransmission"></a>New-MulticastTransmission

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una nuova trasmissione multicast per un'immagine. Questo comando equivale a creare una trasmissione tramite lo snap-in MMC Servizi di distribuzione Windows (fare clic con il pulsante destro del mouse sul nodo **trasmissioni multicast** , quindi fare clic su **Crea trasmissione multicast**). Utilizzare questo comando quando si dispone di Server di trasporto installato il servizio ruolo, ovvero l'installazione predefinita, e il servizio ruolo Server di distribuzione. Se si dispone solo Server di trasporto installato il servizio ruolo, utilizzare [utilizzando il comando nuovo spazio dei nomi](using-the-new-namespace-command.md).
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
per le trasmissioni di immagini di avvio (supportate solo per Windows Server 2008 R2):
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
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media<Image name>|Specifica il nome dell'immagine devono essere trasmessi tramite il multicast.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|FriendlyName<Friendly name>|Specifica il nome descrittivo della trasmissione.|
|/Description<Description>]|Specifica la descrizione della trasmissione.|
MediaType: {boot&#124;install}|Specifica il tipo di immagine devono essere trasmessi tramite il multicast. Nota **avvio** è supportato solo per Windows Server 2008 R2.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini che contiene l'immagine. Se viene specificato alcun nome di gruppo di immagini e il gruppo solo un'immagine esistente sul server, viene utilizzato il gruppo di immagini. Se è presente più di un gruppo di immagini nel server, è necessario utilizzare questa opzione per specificare il nome del gruppo di immagini.|
|[/Filename:<File name>]|specifica il nome file. Se l'immagine di origine non può essere identificata in modo univoco dal nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|/TransmissionType: {AutoCast &#124; multicast}|Specifica se avviare automaticamente la trasmissione (automatico) o in base ai criteri di inizio specificato (multicast).<p><ul><li>**Multicast automatico**. Questo tipo di trasmissione indica che non appena un client richiede un'immagine di installazione, ha inizio la trasmissione multicast dell'immagine selezionata. Se altri client richiedono la stessa immagine, in cui vengono aggiunti la trasmissione che è già stata avviata.</li><li>**Il multicast pianificato**. Questo tipo di trasmissione imposta i criteri per la trasmissione in base al numero di client che richiedono un'immagine e/o un giorno specifico e l'ora di avvio. È possibile specificare le opzioni seguenti:<p><ul><li>[/ora: <time>]-imposta l'ora di inizio della trasmissione usando il formato seguente: aaaa/mm/gg: HH: mm.</li><li>[/ Client: <Number of clients>] -Imposta il numero minimo di client da attendere prima che venga avviata la trasmissione.</li></ul></li></ul>|
|/ Architettura: {x86 & #124; ia64 & #124; x64}|Specifica l'architettura dell'immagine di avvio per la trasmissione mediante il multicast. Poiché è possibile avere lo stesso nome per le immagini di avvio di diverse architetture, è necessario specificare l'architettura per garantire che venga utilizzata l'immagine corretta.|
|[/Filename:<File name>]|specifica il nome file. Se l'immagine di origine non può essere identificata in modo univoco dal nome, è necessario specificare il nome del file.|
## <a name="examples"></a>Esempi
Per creare una trasmissione multicast automatico di un'immagine di avvio in Windows Server 2008 R2, digitare:
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS Boot Transmission
/Image:X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
Per creare una trasmissione multicast automatico di un'immagine di installazione, digitare:
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS AutoCast Transmission
/Image:Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
Per creare una trasmissione multicast pianificato di un'immagine di installazione, digitare:
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS SchedCast Transmission /Server:MyWDSServemedia:Vista with Officemediatype:Install 
/Transmissiontype:ScheduledCast /time:2006/11/20:17:00 /Clients:100
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando[tramite il comando Get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[utilizzando il comando Get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
[utilizzando il comando Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[sottocomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
