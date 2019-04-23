---
title: msinfo32
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3de5088b64105e970fc38f55ecaf54382670549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843462"
---
# <a name="msinfo32"></a>msinfo32

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di aprire lo strumento System Information per visualizzare una panoramica completa dell'ambiente software, i componenti di sistema e hardware del computer locale. 
## <a name="syntax"></a>Sintassi
```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computerName>] [/showcategories] [/category <CategoryID>] [/categories {+<CategoryID>(+<CategoryID>)|+all(-<CategoryID>)}]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<path>|Specifica il file da aprire nel formato *C:\Folder1\File1.XXX*, dove *C* è la lettera dell'unità *Cartella1* è la cartella *File1*è il nome del file, e *XXX* è l'estensione del nome file.<br /><br />Questo file può essere un' **nfo**, **. XML**, **txt**, oppure **CAB** file.|
|<computerName>|Specifica il nome del computer locale o di destinazione. Può trattarsi di un nome UNC, un indirizzo IP o nome completo del computer.|
|<CategoryID>|Specifica l'ID dell'elemento di categoria. È possibile ottenere l'ID della categoria usando **/showcategories**.|
|/pch|Consente di visualizzare la visualizzazione della cronologia di sistema nello strumento di informazioni di sistema.|
|/nfo|Consente di salvare il file esportato come un **nfo** file. Se il nome del file che viene specificato nel *tracciato* non termina con un **nfo** estensione, il **nfo** estensione viene automaticamente aggiunto al nome del file.|
|/report|Salva il file in *percorso* come file di testo. Il nome del file viene salvato esattamente come appare nella *percorso*. L'estensione. txt non viene aggiunta al file, a meno che non è specificato nel percorso.|
|/computer|Avvia lo strumento System Information per il computer remoto specificato. È necessario disporre delle autorizzazioni appropriate per accedere al computer remoto.|
|/showcategories|viene avviato lo strumento System Information con gli ID di categoria tutti disponibili, anziché visualizzare i nomi descrittivi o localizzati. Ad esempio, la categoria dell'ambiente Software viene visualizzata come il **SWEnv** categoria.|
|/category|Avvia le informazioni di sistema con la categoria specificata selezionata. Uso **/showcategories** per visualizzare un elenco di ID categoria disponibili.|
|/categories|Avvia le informazioni di sistema con solo la categoria specificata o categorie visualizzate. Inoltre limita l'output per le categorie o categoria selezionata. Uso **/showcategories** per visualizzare un elenco di ID categoria disponibili.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
Alcune categorie di informazioni di sistema contengono grandi quantità di dati. È possibile usare la **start /wait** comandi per ottimizzare le prestazioni dei report relativi a tali categorie. Per altre informazioni, vedere [le informazioni di sistema](https://technet.microsoft.com/library/cc783305(v=ws.10).aspx).
## <a name="BKMK_Examples"></a>Esempi
Per elencare gli ID categoria disponibili, digitare:
```
msinfo32 /showcategories
```
Per avviare lo strumento System Information con tutte le informazioni disponibili visualizzati, ad eccezione di moduli caricati, digitare:
```
msinfo32 /categories +all -loadedmodules
```
Per visualizzare solo le informazioni di riepilogo del sistema e creare file con estensione nfo chiamato syssum.nfo contenente le informazioni nella categoria di risorse di sistema, digitare:
```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```
Per visualizzare le informazioni sui conflitti di risorse e creare un file con estensione nfo denominato Conflicts. nfo che contiene informazioni sui conflitti di risorse, digitare:
```
msinfo32 /nfo conflicts.nfo /categories    +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)

