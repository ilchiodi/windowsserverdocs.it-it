---
Title: Eseguire la migrazione della replica SYSVOL nella replica DFS
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 30b3c5925971a2709ba422a017a73f47e72813a7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284430"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Eseguire la migrazione della replica SYSVOL nella replica DFS


Aggiornamento: 25 agosto 2010

Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

I controller di dominio usano una cartella condivisa speciale denominata SYSVOL per replicare gli script di accesso e i file oggetto Criteri di gruppo in altri controller di dominio. Windows 2000 Server e Windows Server 2003 usare servizio Replica File (FRS) per replicare SYSVOL, mentre Windows Server 2008 Usa il servizio Replica DFS più recente in domini che utilizzano il livello funzionale di dominio di Windows Server 2008 e FRS per i domini che eseguire i precedenti livelli funzionali del dominio.

Per usare replica DFS per replicare la cartella SYSVOL, è possibile creare un nuovo dominio che utilizza il livello funzionale di dominio di Windows Server 2008 oppure è possibile usare la procedura illustrata in questo documento per eseguire l'aggiornamento di un dominio esistente ed eseguire la migrazione di replica in Replica DFS.

Questo documento si presuppone una conoscenza di base di Active Directory Domain Services (AD DS), FRS e replica DFS (DFS replica). Per altre informazioni, vedere [Panoramica di servizi di dominio di Active Directory](http://go.microsoft.com/fwlink/?linkid=147787), [Cenni preliminari su FRS](http://go.microsoft.com/fwlink/?linkid=121763), o [Panoramica della replica DFS](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Per scaricare una versione stampabile di questa Guida, visitare <a href="http://go.microsoft.com/fwlink/?linkid=150375">Guida alla migrazione di replica di SYSVOL: Replica da FRS a DFS</a> (http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Contenuto della guida

[Informazioni concettuali sulla migrazione di SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [Stati di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Panoramica della procedura di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procedura di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migrazione allo stato preparato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migrazione allo stato reindirizzato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migrazione allo stato eliminato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Risoluzione dei problemi di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Risoluzione dei problemi di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Eseguire il rollback della migrazione SYSVOL a uno stato stabile precedente](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informazioni di riferimento di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Scenari di migrazione SYSVOL supportati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Verificare lo stato di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Azioni dell'utilità di migrazione SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Altri riferimenti

[Serie di migrazione SYSVOL: Parte 1: Introduzione al processo di migrazione SYSVOL](http://go.microsoft.com/fwlink/?linkid=121756)

[Serie di migrazione SYSVOL: Parte 2—Dfsrmig.exe: Lo strumento di migrazione SYSVOL](http://go.microsoft.com/fwlink/?linkid=121757)

[Serie di migrazione SYSVOL: Parte 3: migrazione allo stato 'Preparato'](http://go.microsoft.com/fwlink/?linkid=121758)

[Serie di migrazione SYSVOL: Parte 4: eseguire la migrazione di stato 'REINDIRIZZATO'](http://go.microsoft.com/fwlink/?linkid=121759)

[Serie di migrazione SYSVOL: Parte 5: eseguire la migrazione allo stato 'Eliminato'](http://go.microsoft.com/fwlink/?linkid=121760)

[Guida dettagliata per i sistemi di File distribuito in Windows Server 2008](http://go.microsoft.com/fwlink/?linkid=85231)

[Riferimenti tecnici del servizio Replica file](http://go.microsoft.com/fwlink/?linkid=121764)

