---
title: Prehashing e precaricamento del contenuto sui server cache ospitata (facoltativo)
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b421132a44240520e3e3ba294623584c36b18ab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867002"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Prehashing e precaricamento del contenuto sui server cache ospitata (facoltativo)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per forzare la creazione di informazioni sul contenuto, denominate anche hash: nel server Web e file abilitati per BranchCache. È anche possibile raccogliere i dati nei server web e file in pacchetti che possono essere trasferiti al server cache ospitata remoto.  Questo offre la possibilità di precaricare contenuto nel server cache ospitata remoto in modo che i dati sono disponibili per il primo accesso ai client.  
  
È necessario essere un membro di **amministratori**, o equivalente a eseguire questa procedura.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Prehash contenuto e a precaricare il contenuto nel server cache ospitata  
  
1.  Accedere al server Web che contiene i dati che si desiderano precaricare i file e identificare le cartelle e file che si vuole caricare in uno o più server cache ospitata remoto.  
  
2.  Eseguire Windows PowerShell come amministratore. Per ogni cartella e file, eseguire la `Publish-BCFileContent` comando o `Publish-BCWebContent` comando, a seconda del tipo di server di contenuti, per attivare la generazione di hash e aggiungere dati a un pacchetto di dati.  
  
3.  Dopo aver aggiunto tutti i dati al pacchetto di dati, esportarlo utilizzando la `Export-BCCachePackage` comando per generare un file di pacchetto di dati.  
  
4.  Spostare il file del pacchetto dei dati per i server cache ospitata remoto usando la scelta della tecnologia di trasferimento di file.  FTP, SMB, HTTP, DVD e dischi rigidi portatili sono vitali tutti i trasporti.  
  
5.  Importare il file del pacchetto dei dati nei server cache ospitata remoto usando il `Import-BCCachePackage` comando.  
  

