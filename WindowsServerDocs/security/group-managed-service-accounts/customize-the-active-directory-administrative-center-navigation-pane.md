---
title: Personalizzare il riquadro di spostamento di centro di amministrazione di Active Directory
ms.prod: windows-server-threshold
description: Protezione di Windows Server
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: e7b1128d93912f724225905bedd38131f8aab0b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizzare il riquadro di spostamento di centro di amministrazione di Active Directory

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

  You can browse through the Active Directory Administrative Center navigation pane by using the tree view, which is similar to the Active Directory Users and Computers console tree, or by using the list view.

 Whether you use the tree view or the list view, you can customize your Active Directory Administrative Center navigation pane anytime by adding various containers from the local domain or any foreign domain \(that is, a domain other than the local domain that has an established trust with the local domain\) to the navigation pane as separate nodes. Customizing the Active Directory Administrative Center navigation pane can provide quicker access to Active Directory objects. Per ulteriori informazioni, vedere [gestire domini diversi in Centro di amministrazione di Active Directory ](manage-different-domains-in-active-directory-administrative-center.md).

 Inoltre, per personalizzare ulteriormente il riquadro di spostamento, è possibile rinominare o rimuovere questi nodi riquadro di spostamento aggiunti manualmente, creare duplicati di tali nodi oppure spostarli verso l'alto o verso il basso nel riquadro di spostamento.

> [!NOTE]
>  Non è possibile personalizzare il nodo di dominio locale predefinito.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>To customize the Active Directory Administrative Center navigation pane

1.  In the Active Directory Administrative Center navigation pane, right\-click the node that you want to modify. È possibile modificare la posizione o il nome del nodo oppure è possibile creare un duplicato del.

2.  Fare clic su uno dei seguenti comandi:

    -   **Rinominare**

    -   **Crea nodo duplicato**

    -   **Rimuovere**

    -   **Spostare verso l'alto**

    -   **Spostare verso il basso**

 Utilizzando la visualizzazione elenco, è possibile sfruttare l'elenco MRU \(MRU\). L'elenco viene visualizzato automaticamente sotto il nodo di spostamento quando si visita almeno un contenitore in tale nodo di spostamento. You can also view the current MRU list by expanding the breadcrumb bar at the top of the Active Directory Administrative Center window. L'elenco MRU contiene sempre gli ultimi tre contenitori visitati in un determinato nodo di spostamento. Ogni volta che si seleziona un determinato contenitore, questo contenitore viene aggiunto all'inizio dell'elenco di recente e l'ultimo contenitore visualizzato nell'elenco MRU viene rimosso dall'elenco.

  

