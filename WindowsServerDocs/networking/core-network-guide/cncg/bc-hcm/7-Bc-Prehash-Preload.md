---
title: Eseguire il prehashing e il precaricamento del contenuto nel server della cache ospitata (facoltativo)
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe206576278b09e4a360c7bb27f5ff076af97be7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356248"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Prehash e precaricamento del contenuto nel server cache ospitata \(facoltativo\)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare le procedure descritte in questa sezione per prehash content nei server di contenuti, aggiungere il contenuto ai pacchetti di dati e quindi precaricare il contenuto nei server cache ospitata. 

Queste procedure sono facoltative in quanto non implica la necessità di precaricamento e prehash contenuto nei server cache ospitata. 

Se non si carica il contenuto, i dati vengono aggiunti automaticamente alla cache ospitata quando i client lo scaricano tramite la connessione WAN.

>[!IMPORTANT]
>Sebbene queste procedure siano collettivamente facoltative, se si decide di prehash e precaricare il contenuto nei server cache ospitata, è necessario eseguire entrambe le procedure.

- [Creare pacchetti di dati del server di contenuti per contenuto &#40;Web e file facoltativo&#41;](8-Bc-Data-Packages.md)
  
- [Importare i pacchetti di dati nel server &#40;cache ospitata facoltativo&#41;](9-Bc-Import-Data.md)

Per continuare con questa Guida, vedere [creare dei pacchetti di dati contenuto Server per Web e File di contenuto & #40; facoltativo & #41;](8-Bc-Data-Packages.md).