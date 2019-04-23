---
title: Utilizzando il comando nuovo spazio dei nomi
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50d51101afe95c99b7034fc50b3d30b799ee02ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871152"
---
# <a name="using-the-new-namespace-command"></a>Utilizzando il comando nuovo spazio dei nomi

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/FriendlyName:<Friendly name>|Specifica il nome descrittivo dello spazio dei nomi.|
|[/ Descrizione:<Description>]|Imposta la descrizione dello spazio dei nomi.|
|/ Namespace:<Namespace name>|Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<br /><br />-   **Servizio ruolo Server di distribuzione**: La sintassi per questa opzione è /namespace:<Image group>/<Image name>/<Index>. Ad esempio:  **WDS:ImageGroup1/install.wim/1**<br />-   **Servizio ruolo Server di trasporto**: Questo valore deve corrispondere al nome specificato quando lo spazio dei nomi è stato creato nel server.|
|/ Provider di contenuti:<Name>]|Specifica il nome del provider di contenuti che fornisce contenuto per lo spazio dei nomi.|
|[/ ConfigString:<Configuration string>]|Specifica la stringa di configurazione per il provider di contenuti.|
|/Namespacetype: {AutoCast &#124; ScheduledCast}|Specifica le impostazioni per la trasmissione. Specificare le impostazioni utilizzando le opzioni seguenti:<br /><br />-[/ ora: <time>]-imposta il tempo di trasmissione deve iniziare con il formato seguente: YYYY/MM/DD:hh:mm. Questa opzione è valida solo per le trasmissioni multicast pianificato.<br />-[/ Client: <Number of clients>]-imposta il numero minimo di client da attendere prima che venga avviata la trasmissione. Questa opzione è valida solo per le trasmissioni multicast pianificato.|
## <a name="BKMK_examples"></a>Esempi
Per creare uno spazio dei nomi multicast automatico, digitare:
```
wdsutil /New-Namespace /FriendlyName:"Custom AutoCast Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
Per creare uno spazio dei nomi multicast pianificato, digitare:
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:"Custom Scheduled Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:"2006/11/20:17:00" /Clients:20
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[utilizzando il comando remove-Namespace](using-the-remove-namespace-command.md)
[sottocomando: start-spazio dei nomi](subcommand-start-namespace.md)
