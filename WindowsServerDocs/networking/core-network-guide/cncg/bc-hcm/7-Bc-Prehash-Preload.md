---
title: Prehash e a precaricare contenuto nel Server Cache ospitata (facoltativo)
description: Questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata in computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0e7ffaac4e427222d5539195ecef91768f61c4a3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Prehash e a precaricare contenuto nel contenuto nella Cache Server \(Optional\)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare le procedure in questa sezione per prehash contenuto sui server di contenuti, Aggiungi il contenuto per i pacchetti di dati e quindi precaricare il contenuto nei server cache ospitata. 

Queste procedure sono facoltative perché non si deve prehash e precaricamento contenuto nei server cache ospitata. 

Se non si precaricare contenuto, i dati viene aggiunto alla cache ospitata automaticamente come client scaricano tramite una connessione WAN.

>[!IMPORTANT]
>Anche se queste procedure sono collettivamente facoltative, se si decide di precaricamento e prehash contenuto nei server cache ospitata, è necessario eseguire entrambe le procedure.

- [Creare pacchetti di dati contenuto Server Web e il contenuto del File & #40; facoltativo & #41;](8-Bc-Data-Packages.md)
  
- [Importare i pacchetti di dati nel Server Cache ospitata & #40; facoltativo & #41;](9-Bc-Import-Data.md)

Per continuare con questa Guida, vedere [creare i pacchetti di dati contenuto Server per Web e File di contenuto & #40; facoltativo & #41; ](8-Bc-Data-Packages.md).