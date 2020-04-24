---
title: Eseguire la migrazione della replica SYSVOL nella replica DFS
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: eb7997b4ed16812ac6b1d6c7d3ccc220f8f63ce7
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80815584"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Eseguire la migrazione della replica SYSVOL nella replica DFS


Aggiornamento: 25 agosto 2010

Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

I controller di dominio usano una speciale cartella condivisa denominata SYSVOL per replicare in altri controller di dominio gli script di accesso e i file oggetto Criteri di gruppo. Windows Server 2000 e Windows Server 2003 usano il servizio Replica file per replicare SYSVOL, mentre Windows Server 2008 usa il nuovo servizio Replica DFS nei domini che usano il livello di funzionalità del dominio di Windows Server 2008 e il servizio FRS per i domini che eseguono livelli funzionali di domini precedenti.

Per usare Replica DFS per replicare la cartella SYSVOL, è possibile creare un nuovo dominio che usa il livello funzionale del dominio di Windows Server 2008 oppure la procedura descritta in questo documento per aggiornare un dominio esistente ed eseguire la migrazione della replica a Replica DFS.

In questo documento si presuppone che l'utente disponga di una conoscenza di base di Active Directory Domain Services (AD DS), di FRS e della Replica DFS (Distributed File System). Per altre informazioni, vedere [Panoramica di Active Directory Domain Services](https://go.microsoft.com/fwlink/?linkid=147787), [Panoramica di FRS](https://go.microsoft.com/fwlink/?linkid=121763) o [Panoramica di Replica DFS](https://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Per scaricare una versione stampabile di questa guida, passare a <a href="https://go.microsoft.com/fwlink/?linkid=150375">Guida alla migrazione della replica SYSVOL: dal servizio Replica file a Replica DFS</a> (https://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Contenuto della guida

[Informazioni concettuali relative alla migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [Stati della migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Panoramica della procedura di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procedura di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migrazione allo stato preparato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migrazione allo stato reindirizzato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migrazione allo stato eliminato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Risoluzione dei problemi relativi alla migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Risoluzione dei problemi relativi alla migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Rollback della migrazione SYSVOL a uno stato stabile precedente](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informazioni di riferimento relative alla migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Scenari di migrazione SYSVOL supportati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Verifica dello stato della migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Azioni dell'Utilità di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Altri riferimenti

[Stati della migrazione SYSVOL: Parte 1 - Introduzione al processo di migrazione SYSVOL](https://go.microsoft.com/fwlink/?linkid=121756)

[Stati della migrazione SYSVOL: Part 2 - Dfsrmig.exe: Strumento di migrazione SYSVOL](https://go.microsoft.com/fwlink/?linkid=121757)

[Stati della migrazione SYSVOL: Parte 3 - Migrazione allo stato preparato](https://go.microsoft.com/fwlink/?linkid=121758)

[Stati della migrazione SYSVOL: Parte 4 - Migrazione allo stato reindirizzato](https://go.microsoft.com/fwlink/?linkid=121759)

[Stati della migrazione SYSVOL: Parte 5 - Migrazione allo stato eliminato](https://go.microsoft.com/fwlink/?linkid=121760)

[Guida dettagliata per i file system distribuiti in Windows Server 2008](https://go.microsoft.com/fwlink/?linkid=85231)

[Documentazione tecnica su FRS](https://go.microsoft.com/fwlink/?linkid=121764)

