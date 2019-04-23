---
ms.assetid: fd427da3-3869-428f-bf2a-56c4b7d99b40
title: Clonazione dei blocchi su ReFS
description: ''
author: gawatu
ms.author: gawatu
manager: gawatu
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-file-systems
ms.openlocfilehash: 54165700209320eee50fc63d98d78cbf4a92d053
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838112"
---
# <a name="block-cloning-on-refs"></a>Clonazione dei blocchi su ReFS

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

La clonazione dei blocchi indica al file system di copiare un intervallo di byte di file per conto di un'applicazione in cui il file di destinazione può essere lo stesso oppure può essere diverso dal file di origine. Le operazioni di copia sono costose in quanto attivano letture e scritture impegnative sui dati fisici sottostanti. 

La clonazione dei blocchi in ReFS, tuttavia, esegue le copie come un'operazione di metadati a basso costo invece che leggere e scrivere nei dati dei file. Poiché ReFS consente a più file di condividere gli stessi cluster logici (posizioni fisiche su un volume), le operazioni di copia devono solo rimappare un'area di un file in una posizione fisica separata, trasformando in questo modo un'operazione fisica costosa in una rapida operazione logica. Ciò consente di completare le copie in modo più rapido e di generare meno I/O nell'archiviazione sottostante. Questo miglioramento offre vantaggi anche ai carichi di lavoro della virtualizzazione in quanto, quando si utilizzano le operazioni di clonazione dei blocchi, le operazioni di unione del checkpoint .vhdx. vengono estremamente velocizzate. Inoltre, poiché più file possono condividere gli stessi cluster logici, i dati identici non vengono fisicamente archiviati più volte, migliorando la capacità di archiviazione. 
  
## <a name="how-it-works"></a>Come funziona 

La clonazione dei blocchi su ReFS converte un'operazione dati dei file in un'operazione di metadati. Per effettuare questa ottimizzazione, ReFS introduce nei metadati i conteggi dei riferimenti per le aree copiate. Questo conteggio dei riferimenti registra il numero delle aree distinte del file che si riferiscono alle stesse aree fisiche. Ciò consente a più file di condividere gli stessi dati fisici:

![Mostra gli aggiornamenti del conteggio dei riferimenti quando più file fanno riferimento alla stessa area](media/ref-count-example.gif)

Mantenendo un conteggio dei riferimenti per ogni cluster logico, ReFS non interrompe l'isolamento dei file: la scrittura nelle aree condivise attiva un meccanismo di allocazione in fase di scrittura dove ReFS alloca una nuova area per la scrittura. Questo meccanismo preserva l'integrità dei cluster logici condivisi. 

### <a name="example"></a>Esempio
Si supponga ci siano due file, X e Y; ciascun file è formato da tre aree e ciascuna area è mappata su cluster logici separati.

![Due file, ciascuno con tre aree distinte che mappano tutte su aree che hanno il conteggio dei riferimenti 1](media/block-clone-1.png)

Si supponga ora che un'applicazione avvii un'operazione di clonazione dei blocchi dal File X al File Y per copiare le aree A e B all'offset dell'area E. Lo stato del file system sarà il seguente:

![Il conteggio dei riferimenti indica 2 per l'area con la clonazione dei blocchi](media/block-clone-2.png)

Questo stato del file system indica una duplicazione corretta dell'area con i blocchi clonati. Poiché ReFS effettua questa operazione di copia aggiornando solo le mappature VCN su LCN, non sono stati letti dati fisici, né sovrascritti dati fisici nel File Y. I file X e Y ora condividono cluster logici, riflessi dai conteggi dei riferimenti nella tabella. Poiché non sono stati fisicamente copiati dati, ReFS riduce il consumo di capacità del volume. 

Si supponga ora che l'applicazione tenti di sovrascrivere l'area A nel File X. ReFS duplicherà l'area condivisa, aggiornerà il conteggio dei riferimenti in modo appropriato ed eseguirà la scrittura nell'area duplicata. In questo modo viene mantenuto l'isolamento dei file.   

![L'isolamento viene mantenuto mediante la scrittura in una nuova area G e l'aggiornamento dei conteggi dei riferimenti.](media/block-clone-3.png)

Dopo la scrittura di modifica, l'area B è ancora condivisa da entrambi i file. Notare che se l'area A fosse stata più grande di un cluster, sarebbe stato duplicato solo il cluster modificato e la parte rimanente sarebbe rimasta condivisa.


## <a name="functionality-restrictions-and-remarks"></a>Restrizioni e note sulle funzionalità
- L'area di origine e di destinazione deve iniziare e terminare al limite di un cluster. 
- L'area clonata deve avere una lunghezza minore di 4 GB. 
- Il numero massimo di aree di file che possono mappare sulla stessa area fisica è di 8175.
- L'area di destinazione non può estendersi oltre la fine del file. Se l'applicazione vuole estendere la destinazione con i dati clonati, deve prima chiamare [SetEndOfFile](https://msdn.microsoft.com/library/windows/desktop/aa365531(v=vs.85).aspx). 
- Se le aree di origine e di destinazione si trovano nello stesso file, non si devono sovrapporre. (L'applicazione può continuare dividendo l'operazione di clonazione dei blocchi in più clonazioni dei blocchi che non si sovrappongono).
- I file di origine e di destinazione devono essere nello stesso volume ReFS. 
- I file di origine e di destinazione devono avere la stessa impostazione [Flussi di integrità](https://msdn.microsoft.com/library/windows/desktop/gg258117(v=vs.85).aspx). 
- Se il file di origine è un file sparse, anche il file di destinazione deve essere sparse. 
- L'operazione di clonazione dei blocchi interromperà i blocchi opportunistici condivisi (noti anche come [Blocchi opportunistici di Livello 2](https://msdn.microsoft.com/library/windows/desktop/aa365713(v=vs.85).aspx)).
- Il volume ReFS deve essere stato formattato con Windows Server 2016 e, se è in uso il Clustering di failover, il Livello di funzionalità del clustering al momento della formattazione doveva essere Windows Server 2016 o versioni successive. 

## <a name="see-also"></a>Vedere anche

-   [Cenni preliminari su reFS](refs-overview.md)
-   [Flussi di integrità di reFS](integrity-streams.md)
-   [Panoramica di spazi diretti di archiviazione](../storage-spaces/storage-spaces-direct-overview.md)
-   [DUPLICATE_EXTENTS_DATA](https://msdn.microsoft.com/library/windows/desktop/mt590821(v=vs.85).aspx)
-   [FSCTL_DUPLICATE_EXTENTS_TO_FILE](https://msdn.microsoft.com/library/windows/desktop/mt590823(v=vs.85).aspx)
