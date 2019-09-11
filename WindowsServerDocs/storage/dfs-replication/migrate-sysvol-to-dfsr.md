---
title: Eseguire la migrazione della replica SYSVOL nella replica DFS
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 30222d0737ac3cd2947fc3b2d70a0df77b606299
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871989"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Eseguire la migrazione della replica SYSVOL nella replica DFS


Aggiornamento: 25 agosto 2010

Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

I controller di dominio utilizzano una cartella condivisa speciale denominata SYSVOL per replicare gli script di accesso e Criteri di gruppo file oggetto in altri controller di dominio. Windows Server 2000 e Windows Server 2003 usano il servizio Replica file (FRS) per replicare SYSVOL, mentre Windows Server 2008 usa il servizio Replica DFS più recente nei domini che usano il livello di funzionalità del dominio Windows Server 2008 e FRS per i domini che eseguire i livelli di funzionalità del dominio precedenti.

Per usare Replica DFS per replicare la cartella SYSVOL, è possibile creare un nuovo dominio che usa il livello di funzionalità del dominio di Windows Server 2008. in alternativa, è possibile usare la procedura descritta in questo documento per aggiornare un dominio esistente ed eseguire la migrazione della replica a Replica DFS.

In questo documento si presuppone che l'utente disponga di una conoscenza di base di Active Directory Domain Services (AD DS), FRS e della replica di file system distribuito (Replica DFS). Per ulteriori informazioni, vedere Panoramica di [Active Directory Domain Services](http://go.microsoft.com/fwlink/?linkid=147787), [Panoramica di FRS](http://go.microsoft.com/fwlink/?linkid=121763)o [Panoramica di replica DFS](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Per scaricare una versione stampabile di questa guida, vedere Guida <a href="http://go.microsoft.com/fwlink/?linkid=150375">alla migrazione della replica di SYSVOL: FRS per Replica DFS</a> (http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Contenuto della guida

[Informazioni concettuali sulla migrazione di SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [Stati di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Cenni preliminari sulla procedura di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procedura di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migrazione allo stato preparato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migrazione allo stato reindirizzato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migrazione allo stato eliminato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Risoluzione dei problemi relativi alla migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Risoluzione dei problemi di migrazione di SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Rollback della migrazione di SYSVOL a uno stato stabile precedente](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informazioni di riferimento sulla migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Scenari di migrazione SYSVOL supportati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Verifica dello stato della migrazione di SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Azioni dello strumento di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Altri riferimenti

[Serie di migrazione SYSVOL: Parte 1: Introduzione al processo di migrazione SYSVOL](http://go.microsoft.com/fwlink/?linkid=121756)

[Serie di migrazione SYSVOL: Parte 2-dfsrmig. exe: Strumento di migrazione SYSVOL](http://go.microsoft.com/fwlink/?linkid=121757)

[Serie di migrazione SYSVOL: Parte 3-migrazione allo stato ' preparato '](http://go.microsoft.com/fwlink/?linkid=121758)

[Serie di migrazione SYSVOL: Parte 4: migrazione allo stato ' Redirected '](http://go.microsoft.com/fwlink/?linkid=121759)

[Serie di migrazione SYSVOL: Parte 5: migrazione allo stato ' eliminato '](http://go.microsoft.com/fwlink/?linkid=121760)

[Guida dettagliata per i file system distribuiti in Windows Server 2008](http://go.microsoft.com/fwlink/?linkid=85231)

[Riferimento tecnico per FRS](http://go.microsoft.com/fwlink/?linkid=121764)

