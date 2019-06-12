---
title: Personalizzare il riquadro di spostamento di centro di amministrazione di Active Directory
ms.prod: windows-server-threshold
description: Sicurezza di Windows Server
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: a4cab0246226cf22a1b7212b832a902783952407
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446501"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizzare il riquadro di spostamento di centro di amministrazione di Active Directory

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

  È possibile esplorare il riquadro di spostamento di centro di amministrazione di Active Directory utilizzando la visualizzazione struttura ad albero, che è simile alla struttura della console Active Directory Users and Computers, oppure utilizzando la visualizzazione elenco.

 Se si usa la visualizzazione struttura ad albero o della visualizzazione elenco, è possibile personalizzare in qualsiasi momento il riquadro di spostamento di centro di amministrazione di Active Directory mediante l'aggiunta di diversi contenitori dal dominio locale o di qualsiasi dominio esterno \(, ovvero un dominio diverso dal dominio locale che ha una relazione di trust stabilita con il dominio locale\) al riquadro di spostamento come nodi separati. Personalizzazione del riquadro di spostamento di centro di amministrazione di Active Directory, è possibile fornire un accesso più rapido per gli oggetti Active Directory. Per altre informazioni, vedere [gestire domini diversi nel centro di amministrazione di Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Per personalizzare ulteriormente il riquadro di spostamento, è inoltre possibile rinominare o rimuovere i nodi aggiunti manualmente al riquadro di spostamento, creare duplicati di tali nodi oppure spostarli verso l'alto o verso il basso nel riquadro di spostamento.

> [!NOTE]
>  Non è possibile personalizzare il nodo del dominio locale predefinito.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Per personalizzare il riquadro di spostamento di centro di amministrazione di Active Directory

1. Nel riquadro di spostamento di centro di amministrazione di Active Directory, con il pulsante destro\-fare clic sul nodo che si desidera modificare. È possibile modificare la posizione o il nome del nodo oppure è possibile creare un duplicato del nodo.

2. Fare clic su uno dei seguenti comandi:

   -   **Rinomina**

   -   **Crea nodo duplicato**

   -   **Rimuovi**

   -   **Spostare verso l'alto**

   -   **Sposta giù**

   Utilizzando la visualizzazione elenco, è possibile sfruttare l'usati di recente \(MRU\) elenco. L'elenco viene visualizzato automaticamente sotto un nodo di navigazione quando si visita almeno un contenitore in questo nodo di navigazione. È anche possibile visualizzare l'elenco MRU corrente espandendo la barra di navigazione nella parte superiore della finestra di centro di amministrazione di Active Directory. L'elenco MRU contiene sempre gli ultimi tre contenitori visitati in un determinato nodo di spostamento. Ogni volta che si seleziona un determinato contenitore, tale contenitore viene aggiunto al primo posto dell'elenco dei contenitori utilizzati più di recente e l'ultimo contenitore visualizzato nell'elenco viene rimosso dall'elenco.

  

