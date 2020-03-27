---
title: Eseguire il prehashing e il precaricamento del contenuto nel server della cache ospitata (facoltativo)
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f47fbb5c10828d16a18b5cf486e620b3ace458c7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318453"
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