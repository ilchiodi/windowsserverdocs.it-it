---
title: Prehashing e precaricamento del contenuto nei server Cache ospitata (facoltativo)
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c3d1ed62c6dca5b1de0ff560fde0a2e43ed0d080
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Prehashing e precaricamento del contenuto nei server Cache ospitata (facoltativo)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per forzare la creazione di informazioni sul contenuto, denominate anche hash - sul server Web e file abilitati per BranchCache. È anche possibile raccogliere i dati nel server web e file in pacchetti che possono essere trasferiti ai server cache ospitata remoto.  Questo offre la possibilità di precaricare contenuto nel server cache ospitata remoto in modo che i dati sono disponibili per il primo accesso client.  
  
È necessario essere un membro del **amministratori**, o equivalente a eseguire questa procedura.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Prehash contenuto e a precaricare il contenuto nei server cache ospitata  
  
1.  Accedere al server Web che contiene i dati che si desiderano precaricare i file e identificare le cartelle e file che si desiderano caricare in uno o più server cache ospitata remoto.  
  
2.  Eseguire Windows PowerShell come amministratore. Per ogni cartella e file, eseguire il `Publish-BCFileContent`comando o `Publish-BCWebContent`comando, a seconda del tipo di server di contenuti, in modo da attivare generazione hash e per aggiungere dati a un pacchetto di dati.  
  
3.  Dopo l'aggiunta di tutti i dati al pacchetto di dati, esportarlo con la `Export-BCCachePackage`comando per produrre un file di pacchetto di dati.  
  
4.  Spostare il file del pacchetto di dati per i server cache ospitata remoto utilizzando la scelta della tecnologia di trasferimento di file.  FTP, SMB, HTTP, DVD e dischi rigidi portatili sono tutte validi trasporti.  
  
5.  Importare il file del pacchetto di dati nei server cache ospitata remoto utilizzando il `Import-BCCachePackage`comando.  
  

