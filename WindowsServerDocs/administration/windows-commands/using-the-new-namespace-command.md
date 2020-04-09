---
title: nuovo-spazio dei nomi
description: Windows Commands argomento per New-Namespace, che consente di creare e configurare un nuovo spazio dei nomi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f616c33525d033827d52925e07764761e7c7b44
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830634"
---
# <a name="new-namespace"></a>nuovo-spazio dei nomi

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea e configura un nuovo spazio dei nomi. Utilizzare questa opzione quando si dispone solo Server di trasporto installato il servizio ruolo. Se si dispone sia il servizio ruolo Server di distribuzione e il servizio ruolo Server di trasporto installato (ovvero l'impostazione predefinita), utilizzare [utilizzando il comando /New-multicasttransmission](using-the-new-multicasttransmission-command.md). Si noti che è necessario registrare il provider di contenuti prima di utilizzare questa opzione.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /New-Namespace [/Server:<Server name>]
     /FriendlyName:<Friendly name>
     [/Description:<Description>]
     /Namespace:<Namespace name>
     /ContentProvider:<Name>
     [/ConfigString:<Configuration string>]
     /Namespacetype: {AutoCast | ScheduledCast}
         [/time:<YYYY/MM/DD:hh:mm>]
         [/Clients:<Number of clients>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/FriendlyName:<Friendly name>|Specifica il nome descrittivo dello spazio dei nomi.|
|/Description<Description>]|Imposta la descrizione dello spazio dei nomi.|
|/Namespace:<Namespace name>|Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<p>-   **servizio ruolo server di distribuzione**: la sintassi per questa opzione è/namespace: WDS:<Image group>/<Image name>/<Index>. Ad esempio: **WDS:ImageGroup1/install.wim/1**<br />**servizio ruolo server di trasporto**-   : questo valore deve corrispondere al nome specificato quando lo spazio dei nomi è stato creato nel server.|
|/ Provider di contenuti:<Name>]|Specifica il nome del provider di contenuti che fornisce contenuto per lo spazio dei nomi.|
|[/ ConfigString:<Configuration string>]|Specifica la stringa di configurazione per il provider di contenuti.|
|/NamespaceType: {AutoCast &#124; multicast}|Specifica le impostazioni per la trasmissione. Specificare le impostazioni utilizzando le opzioni seguenti:<p>-[/ora: <time>]-imposta l'ora di inizio della trasmissione usando il formato seguente: AAAA/MM/GG: HH: mm. Questa opzione è valida solo per le trasmissioni multicast pianificato.<br />-[/ Client: <Number of clients>]-imposta il numero minimo di client da attendere prima che venga avviata la trasmissione. Questa opzione è valida solo per le trasmissioni multicast pianificato.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per creare uno spazio dei nomi multicast automatico, digitare:
```
wdsutil /New-Namespace /FriendlyName:Custom AutoCast Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
Per creare uno spazio dei nomi multicast pianificato, digitare:
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:Custom Scheduled Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:2006/11/20:17:00 /Clients:20
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[utilizzando il comando remove-Namespace](using-the-remove-namespace-command.md)
[sottocomando: start-spazio dei nomi](subcommand-start-namespace.md)
