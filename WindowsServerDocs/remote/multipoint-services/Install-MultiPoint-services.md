---
title: Installare MultiPoint Services
description: Informazioni su come installare e configurare MultiPoint Services in Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 442699afe40ee67e4cd4f13572d1a482f675b84a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395377"
---
# <a name="install-multipoint-services"></a>Installare MultiPoint Services
Se si sta installando un server da zero, seguire queste istruzioni per installare MultiPoint Services.  

Dopo aver installato Windows Server 2016, accedere come amministratore. Usare il Server Manager in cui è possibile abilitare MultiPoint Services. Il Server Manager viene aperto automaticamente all'avvio. Nel Dashboard selezionare **Aggiungi ruoli e funzionalità** per abilitare multipoint Services e seguire le istruzioni della procedura guidata.

Nella sezione relativa al tipo di installazione è possibile usare 
- Installazione basata su ruoli o basata su funzionalità o
- Installazione Servizi Desktop remoto

Per le distribuzioni di MultiPoint Services standard, è consigliabile selezionare l'installazione di Servizi Desktop remoto che consente di selezionare comodamente il ruolo Servizi MultiPoint in tipo di distribuzione. Per l'installazione basata su ruoli è necessario selezionare **multipoint Services** nell'elenco dei ruoli. Il server viene riavviato dopo l'installazione completata.  
  
## <a name="configure-your-primary-station"></a>Configurare la stazione primaria  
  
1.  Nella pagina **Crea una stazione MultiPoint Server** Digitare la lettera specificata dalla tastiera per il monitoraggio. La voce chiave corretta associa la tastiera e il mouse per la stazione.  
2.  Accedere come amministratore.  