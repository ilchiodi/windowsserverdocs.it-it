---
title: Sincronizzare il file server con il cloud usando Sincronizzazione file di Azure
description: USA Sincronizzazione file di Azure per centralizzare le condivisioni file dell'organizzazione in Azure, mantenendo al tempo stesso la flessibilità, le prestazioni e la compatibilità di un file server locale. Sincronizzazione file di Azure trasforma Windows Server in una cache rapida della condivisione file di Azure con la funzionalità facoltativa di suddivisione in livelli nel cloud.
ms.technology: manage
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: fe0e3337962b7d9c2f025a9d4eba826f3349c1f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357379"
---
# <a name="sync-your-file-server-with-the-cloud-by-using-azure-file-sync"></a>Sincronizzare il file server con il cloud usando Sincronizzazione file di Azure

>Si applica a: Windows Admin Center, Windows Admin Center Preview

USA Sincronizzazione file di Azure per centralizzare le condivisioni file dell'organizzazione in Azure, mantenendo al tempo stesso la flessibilità, le prestazioni e la compatibilità di un file server locale. Sincronizzazione file di Azure trasforma Windows Server in una cache rapida della condivisione file di Azure con la funzionalità facoltativa di suddivisione in livelli nel cloud. È possibile usare qualsiasi protocollo disponibile in Windows Server per accedere ai dati localmente, tra cui SMB, NFS e FTPS.

Una volta che i file sono stati sincronizzati con il cloud, è possibile connettere più server alla stessa condivisione file di Azure per la sincronizzazione e la memorizzazione nella cache del contenuto localmente, anche le autorizzazioni (ACL) vengono sempre trasportate. File di Azure offre una funzionalità di snapshot che può generare snapshot differenziali della condivisione file di Azure. Questi snapshot possono essere montati anche come unità di rete di sola lettura tramite SMB per semplificare l'esplorazione e il ripristino. In combinazione con la suddivisione in livelli nel cloud, l'esecuzione di un file server locale non è mai stata così semplice.

Per ulteriori informazioni, vedere [pianificazione di una distribuzione di sincronizzazione file di Azure](https://aka.ms/afs).