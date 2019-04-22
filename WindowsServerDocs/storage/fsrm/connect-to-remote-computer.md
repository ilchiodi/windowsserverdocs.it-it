---
title: Connettersi a un computer remoto
description: Questo articolo descrive come connettersi a un computer remoto per gestire le risorse di archiviazione da Gestione risorse file server
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 93d2be926437b65ed8eb84a828ea0d7da6a51086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818862"
---
# <a name="connect-to-a-remote-computer"></a>Connettersi a un computer remoto 

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Per gestire le risorse di archiviazione su un computer remoto, è possibile connettersi al computer da Gestione risorse file server. Mentre si è connessi, Gestione risorse file server consente di gestire le quote, eseguire lo screening dei file, gestire le classificazioni, pianificare le attività di gestione dei file e generare rapporti con tali risorse remote.

> [!Note]
> Gestione risorse file server è in grado di gestire le risorse sia su un computer locale che un computer remoto, ma non su entrambi allo stesso tempo.

## <a name="to-connect-to-a-remote-computer-from-file-server-resource-manager"></a>Per connettersi a un computer remoto da Gestione risorse file server

1.  In **Strumenti di amministrazione**, fare clic su **Gestione risorse file server**.

2.  Nell'albero della console, fare clic con il pulsante destro del mouse su **Gestione risorse file server**, quindi fare clic su **Connetti a un altro computer**.

3.  Nella finestra di dialogo **Connetti a un altro computer**, fare clic su **Altro computer**. Digitare il nome del server a cui si desidera connettersi (o fare clic su **Sfoglia** per cercare un computer remoto).

4.  Fare clic su **OK**.

> [!Important]
> Il comando **Connetti a un altro computer** è disponibile solo quando si apre Gestione risorse file server da **Strumenti di amministrazione**. Quando si accede a Gestione risorse file server da Gestione server, il comando non è disponibile.

## <a name="additional-considerations"></a>Considerazioni aggiuntive

Per gestire risorse remote con Gestione risorse file server:

-   È necessario effettuare l'accesso al computer locale con un account di dominio che sia membro del gruppo **Amministratori** sul computer remoto.
-   Sul computer remoto deve essere in esecuzione Windows Server e deve essere installato Gestione risorse file server.
-   L'eccezione **Gestione remota Gestione risorse file server** sul computer remoto deve essere abilitata. Abilitare questa eccezione tramite Windows Firewall nel Pannello di controllo.

## <a name="see-also"></a>Vedere anche

-   [La gestione delle risorse di archiviazione remota](managing-remote-storage-resources.md)