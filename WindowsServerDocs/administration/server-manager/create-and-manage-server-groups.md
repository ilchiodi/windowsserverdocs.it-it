---
title: creare e gestire gruppi di Server
description: Server Manager
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32e20040e2cb075e447c0d03d48676c7011a5a92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889482"
---
# <a name="create-and-manage-server-groups"></a>creare e gestire gruppi di Server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene descritto come creare gruppi personalizzati, definiti dall'utente di server in Server Manager in Windows Server.

## <a name="BKMK_groups"></a>Gruppi di server
In vengono visualizzati i server aggiunti al pool di server di **tutti i server** pagina in Server Manager. È possibile creare gruppi personalizzati di server aggiunti. Gruppi di server consentono di visualizzare e gestire un sottoinsieme più piccolo del pool di server come un'unità logica; ad esempio, è possibile creare un gruppo denominato **server contabilità** per tutti i server nell'organizzazione del reparto contabilità, o un gruppo denominato **Chicago** per tutti i server situati geograficamente a Chicago. Dopo aver creato un gruppo di server, home page del gruppo in Server Manager visualizza le informazioni su eventi, servizi, contatori delle prestazioni, risultati di Best Practices Analyzer e installati ruoli e funzionalità per il gruppo nel suo complesso.

I server possono essere membri di più gruppi.

#### <a name="to-create-a-new-server-group"></a>Per creare un nuovo gruppo di server

1.  Nel **Manage** menu, fare clic su **Crea gruppo di Server**.

2.  Nella casella di testo **Nome gruppo server** digitare un nome descrittivo per il gruppo di server, ad esempio **Server contabilità**.

3.  aggiungere server al **selezionati** elencati dal pool di server oppure aggiungere altri server al gruppo tramite il **active directory**, **DNS**, o **importare**schede. Per altre informazioni su come utilizzare queste schede, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md) in questa Guida.

4.  Dopo aver completato l'aggiunta di server al gruppo, fare clic su **OK**. Il nuovo gruppo viene visualizzato nel riquadro di spostamento di Server Manager sotto il **tutti i server** gruppo.

#### <a name="to-edit-an-existing-server-group"></a>Per modificare un gruppo di server esistente

1.  Effettuare una delle operazioni seguenti.

    -   Nel riquadro di spostamento di Server Manager, fare doppio clic su un gruppo di server e quindi fare clic su **Modifica gruppo di Server**.

    -   Nella home page del gruppo di server, aprire il **attività** menu le **server** riquadro e quindi fare clic su **Modifica gruppo di Server**.

2.  modificare il nome del gruppo, oppure aggiungere o rimuovere server dal gruppo.

    > [!NOTE]
    > rimozione di server da un gruppo di server non vengono rimossi da Server Manager. ma rimangono nel gruppo **Tutti i server** nel pool di server.

3.  Dopo aver completato le modifiche del gruppo, fare clic su **OK**.

#### <a name="to-delete-an-existing-server-group"></a>Per eliminare un gruppo di server esistente

1.  Effettuare una delle operazioni seguenti.

    -   Nel riquadro di spostamento di Server Manager, fare doppio clic su un gruppo di server e quindi fare clic su **eliminare il gruppo di Server**.

    -   Nella home page del gruppo di server, aprire il **attività** menu il **server** riquadro e quindi fare clic su **eliminare il gruppo di Server**.

2.  Quando viene richiesto di confermare se si desidera eliminare il gruppo di server fare clic su **Sì**.

    > [!NOTE]
    > l'eliminazione di un gruppo di server non vengono rimossi da Server Manager. ma rimangono nel gruppo **Tutti i server** nel pool di server.

3.  Dopo aver completato le modifiche del gruppo, fare clic su **OK**.

## <a name="see-also"></a>Vedere anche
[aggiungere server a Server Manager](add-servers-to-server-manager.md)
[Server Manager](server-manager.md)



