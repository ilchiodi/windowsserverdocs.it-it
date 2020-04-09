---
title: Installare MultiPoint Services
description: Informazioni su come installare e configurare MultiPoint Services in Windows Server 2016
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: ab82782f4ac1ffa8532dc23ad9340329a65765de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859214"
---
# <a name="install-multipoint-services"></a>Installare MultiPoint Services
Se si sta installando un server da zero, seguire queste istruzioni per installare MultiPoint Services.  

Dopo aver installato Windows Server 2016, accedere come amministratore. Usare il Server Manager in cui è possibile abilitare MultiPoint Services. Il Server Manager viene aperto automaticamente all'avvio. Nel Dashboard selezionare **Aggiungi ruoli e funzionalità** per abilitare multipoint Services e seguire le istruzioni della procedura guidata.

Nella sezione relativa al tipo di installazione è possibile usare 
- Installazione basata su ruoli o basata su funzionalità o
- Installazione di Servizi Desktop remoto

Per le distribuzioni di MultiPoint Services standard, è consigliabile selezionare l'installazione di Servizi Desktop remoto che consente di selezionare comodamente il ruolo Servizi MultiPoint in tipo di distribuzione. Per l'installazione basata su ruoli è necessario selezionare **multipoint Services** nell'elenco dei ruoli. Il server viene riavviato dopo l'installazione completata.  
  
## <a name="configure-your-primary-station"></a>Configurare la stazione primaria  
  
1.  Nella pagina **Crea una stazione MultiPoint Server** Digitare la lettera specificata dalla tastiera per il monitoraggio. La voce chiave corretta associa la tastiera e il mouse per la stazione.  
2.  Accedere come amministratore.  