---
title: Prehashing e precaricamento del contenuto sui server cache ospitata (facoltativo)
description: Questo argomento fa parte della Guida alla distribuzione di BranchCache per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitata per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7fe43b3a7c8dc7906e678a219b67ed096aa951d4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319121"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Prehashing e precaricamento del contenuto sui server cache ospitata (facoltativo)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questa procedura per forzare la creazione di informazioni sul contenuto, denominate anche hash, in server Web e file server abilitati per BranchCache. È anche possibile raccogliere i dati su file e server Web in pacchetti che possono essere trasferiti a server cache ospitata remota.  Questo consente di precaricare il contenuto nei server cache ospitata remota in modo che i dati siano disponibili per il primo accesso client.  
  
È necessario essere un membro di **amministratori**, o equivalente a eseguire questa procedura.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Per prehash Content e precaricare il contenuto nei server cache ospitata  
  
1.  Accedere al file o al server Web che contiene i dati che si desidera precaricare e identificare le cartelle e i file che si desidera caricare in uno o più server cache ospitata remota.  
  
2.  Eseguire Windows PowerShell come amministratore. Per ogni cartella e file, eseguire il comando `Publish-BCFileContent` o il comando `Publish-BCWebContent`, a seconda del tipo di server di contenuti, per attivare la generazione di hash e per aggiungere dati a un pacchetto di dati.  
  
3.  Quando tutti i dati sono stati aggiunti al pacchetto di dati, esportarli usando il comando `Export-BCCachePackage` per produrre un file di pacchetto di dati.  
  
4.  Spostare il file del pacchetto di dati nei server cache ospitata remota utilizzando la tecnologia di trasferimento file scelta.  I dischi rigidi FTP, SMB, HTTP, DVD e portatili sono tutti trasporti possibili.  
  
5.  Importare il file del pacchetto di dati nei server cache ospitata remota utilizzando il comando `Import-BCCachePackage`.  
  

