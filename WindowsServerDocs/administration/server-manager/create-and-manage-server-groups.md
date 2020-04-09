---
title: creare e gestire gruppi di server
description: Server Manager
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f4ad512c55bcd1391ad55bdbdeb9a2ba3bfd7f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851544"
---
# <a name="create-and-manage-server-groups"></a>creare e gestire gruppi di server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene descritto come creare gruppi personalizzati, definiti dall'utente di server in Server Manager in Windows Server.

## <a name="server-groups"></a><a name=BKMK_groups></a>Gruppi di server
In vengono visualizzati i server aggiunti al pool di server di **tutti i server** pagina in Server Manager. È possibile creare gruppi personalizzati di server aggiunti. Gruppi di server consentono di visualizzare e gestire un sottoinsieme più piccolo del pool di server come un'unità logica; ad esempio, è possibile creare un gruppo denominato **server contabilità** per tutti i server nell'organizzazione del reparto contabilità, o un gruppo denominato **Chicago** per tutti i server situati geograficamente a Chicago. Dopo aver creato un gruppo di server, home page del gruppo in Server Manager visualizza le informazioni su eventi, servizi, contatori delle prestazioni, risultati di Best Practices Analyzer e installati ruoli e funzionalità per il gruppo nel suo complesso.

I server possono essere membri di più gruppi.

#### <a name="to-create-a-new-server-group"></a>Per creare un nuovo gruppo di server

1.  Scegliere **Crea gruppo di server**dal menu **Gestisci** .

2.  Nella casella di testo **Nome gruppo server** digitare un nome descrittivo per il gruppo di server, ad esempio **Server contabilità**.

3.  aggiungere i server all'elenco **selezionato** dal pool di server oppure aggiungere altri server al gruppo tramite le schede **Active Directory**, **DNS**o **Importa** . Per ulteriori informazioni su come utilizzare queste schede, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md) in questa guida.

4.  Dopo aver completato l'aggiunta di server al gruppo, fare clic su **OK**. Il nuovo gruppo viene visualizzato nel riquadro di spostamento di Server Manager sotto il **tutti i server** gruppo.

#### <a name="to-edit-an-existing-server-group"></a>Per modificare un gruppo di server esistente

1.  Effettuare una delle operazioni riportate di seguito.

    -   Nel riquadro di spostamento Server Manager fare clic con il pulsante destro del mouse su un gruppo di server e quindi scegliere **modifica gruppo di server**.

    -   Nella home page per il gruppo di server, aprire il menu **attività** nel riquadro **Server** e quindi fare clic su **modifica gruppo di server**.

2.  modificare il nome del gruppo oppure aggiungere o rimuovere server dal gruppo.

    > [!NOTE]
    > la rimozione di server da un gruppo di server non comporta la rimozione di server da Server Manager. ma rimangono nel gruppo **Tutti i server** nel pool di server.

3.  Dopo aver completato le modifiche del gruppo, fare clic su **OK**.

#### <a name="to-delete-an-existing-server-group"></a>Per eliminare un gruppo di server esistente

1.  Effettuare una delle operazioni riportate di seguito.

    -   Nel riquadro di spostamento Server Manager fare clic con il pulsante destro del mouse su un gruppo di server e quindi scegliere **Elimina gruppo di server**.

    -   Nella home page per il gruppo di server, aprire il menu **attività** nel riquadro **Server** e quindi fare clic su **Elimina gruppo di server**.

2.  Quando viene richiesto di confermare se si desidera eliminare il gruppo di server fare clic su **Sì**.

    > [!NOTE]
    > l'eliminazione di un gruppo di server non comporta la rimozione di server da Server Manager. ma rimangono nel gruppo **Tutti i server** nel pool di server.

3.  Dopo aver completato le modifiche del gruppo, fare clic su **OK**.

## <a name="see-also"></a>Vedi anche
[Aggiungi server a Server Manager](add-servers-to-server-manager.md)
[Server Manager](server-manager.md)



