---
title: Personalizzare il riquadro di spostamento Centro di amministrazione di Active Directory
ms.prod: windows-server
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
ms.openlocfilehash: 63038014207acd3846cb8db20c7836718615df51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403760"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizzare il riquadro di spostamento Centro di amministrazione di Active Directory

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

  È possibile esplorare il Centro di amministrazione di Active Directory riquadro di spostamento utilizzando la visualizzazione albero, che è simile all'albero della console utenti e computer Active Directory oppure utilizzando la visualizzazione elenco.

 Se si usa la visualizzazione albero o la visualizzazione elenco, è possibile personalizzare il riquadro di spostamento di Centro di amministrazione di Active Directory in qualsiasi momento aggiungendo diversi contenitori dal dominio locale o da qualsiasi \(di dominio esterno, ovvero un dominio diverso dal dominio locale con una relazione di trust stabilita con il dominio locale\) al riquadro di spostamento come nodi separati. La personalizzazione del riquadro di spostamento Centro di amministrazione di Active Directory può consentire un accesso più rapido agli oggetti Active Directory. Per ulteriori informazioni, vedere [gestire domini diversi in centro di amministrazione di Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Per personalizzare ulteriormente il riquadro di spostamento, è inoltre possibile rinominare o rimuovere i nodi aggiunti manualmente al riquadro di spostamento, creare duplicati di tali nodi oppure spostarli verso l'alto o verso il basso nel riquadro di spostamento.

> [!NOTE]
>  Non è possibile personalizzare il nodo del dominio locale predefinito.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Per personalizzare il riquadro di spostamento Centro di amministrazione di Active Directory

1. Nel riquadro di spostamento Centro di amministrazione di Active Directory\-fare clic con il pulsante destro del mouse sul nodo che si desidera modificare. È possibile modificare la posizione o il nome del nodo oppure è possibile creare un duplicato del nodo.

2. Fare clic su uno dei comandi seguenti:

   -   **Rinominare**

   -   **Crea nodo duplicato**

   -   **Rimuovi**

   -   **Sposta su**

   -   **Sposta giù**

   Utilizzando la visualizzazione elenco, è possibile sfruttare i vantaggi dell'elenco dei\) usati più di recente \(MRU. L'elenco MRU viene visualizzato automaticamente sotto un nodo di navigazione quando si visita almeno un contenitore in questo nodo di navigazione. È anche possibile visualizzare l'elenco MRU corrente espandendo la barra di navigazione nella parte superiore della finestra di Centro di amministrazione di Active Directory. L'elenco MRU contiene sempre gli ultimi tre contenitori visitati in un particolare nodo di navigazione. Ogni volta che si seleziona un determinato contenitore, tale contenitore viene aggiunto al primo posto dell'elenco dei contenitori utilizzati più di recente e l'ultimo contenitore visualizzato nell'elenco viene rimosso dall'elenco.

  

