---
title: Sincronizzare il server di file con il cloud tramite sincronizzazione di File di Azure
description: Usare la sincronizzazione di File Azure di centralizzare le condivisioni di file dell'organizzazione in Azure, pur mantenendo la flessibilità, prestazioni e compatibilità di un file server in locale. Sincronizzazione di File Azure Trasforma Windows Server in una cache rapida della condivisione di file Azure con la funzionalità facoltativa cloud collegamento a livelli.
ms.technology: manage
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cfa469a3c14410d95b0b224cbf59b913ec9f7e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296971"
---
# Sincronizzare il server di file con il cloud tramite sincronizzazione di File di Azure

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Usare la sincronizzazione di File Azure di centralizzare le condivisioni di file dell'organizzazione in Azure, pur mantenendo la flessibilità, prestazioni e compatibilità di un file server in locale. Sincronizzazione di File Azure Trasforma Windows Server in una cache rapida della condivisione di file Azure con la funzionalità facoltativa cloud collegamento a livelli. È possibile utilizzare qualsiasi protocollo che è disponibile in Windows Server per accedere ai dati in locale, inclusi SMB, NFS e FTPS.

Una volta che i file sono sincronizzati nel cloud, puoi connetterti più server per la stessa condivisione di file di Azure per sincronizzare e memorizzare nella cache il contenuto in locale, ovvero le autorizzazioni (ACL) sono sempre trasportate anche. File di Azure offre una funzionalità di snapshot in grado di generare differenziale snapshot della condivisione di file Azure. Queste istantanee possono anche essere montate come unità di rete di sola lettura tramite SMB per semplificare l'esplorazione e ripristino. In combinazione con cloud tiering, che esegue un file server locale è mai stato così semplice.

Per altre informazioni, vedere [pianificazione di una distribuzione di sincronizzazione di File di Azure](https://aka.ms/afs).