---
title: msinfo32
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a77cf9ae4c5f054e66839ff5b5b057e031b36ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839114"
---
# <a name="msinfo32"></a>msinfo32

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Apre lo strumento informazioni di sistema per visualizzare una visualizzazione completa dell'hardware, dei componenti di sistema e dell'ambiente software nel computer locale. 
## <a name="syntax"></a>Sintassi
```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computerName>] [/showcategories] [/category <CategoryID>] [/categories {+<CategoryID>(+<CategoryID>)|+all(-<CategoryID>)}]
```
#### <a name="parameters"></a>Parametri

|    Parametro    |                                                                                                                                 Descrizione                                                                                                                                  |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <path>      | Specifica il file da aprire nel formato *C:\Folder1\File1.xxx*, dove *C* è la lettera di unità, *folder1* è la cartella, *file1* è il nome del file e *xxx* è l'estensione del nome file.<p>Questo file può essere un file con estensione **NFO**, **XML**, **txt**o **CAB** . |
| <computerName>  |                                                                             Specifica il nome del computer locale o di destinazione. Può trattarsi di un nome UNC, di un indirizzo IP o di un nome di computer completo.                                                                              |
|  <CategoryID>   |                                                                                     Specifica l'ID dell'elemento category. È possibile ottenere l'ID di categoria usando **/showcategories**.                                                                                      |
|      /pch       |                                                                                                       Visualizza la visualizzazione della cronologia di sistema nello strumento informazioni di sistema.                                                                                                       |
|      /nfo       |                                     Salva il file esportato come file con **estensione NFO** . Se il nome file specificato nel *percorso* non termina con l'estensione **NFO** , l'estensione **NFO** viene accodata automaticamente al nome del file.                                      |
|     /report     |                                               Salva il file nel *percorso* come file di testo. Il nome del file viene salvato esattamente come viene visualizzato in *path*. L'estensione txt non viene accodata al file, a meno che non sia specificata nel percorso.                                                |
|    /computer    |                                                                avvia lo strumento informazioni di sistema per il computer remoto specificato. È necessario disporre delle autorizzazioni appropriate per accedere al computer remoto.                                                                |
| /showcategories |                         avvia lo strumento informazioni di sistema con tutti gli ID di categoria disponibili visualizzati, anziché visualizzare i nomi descrittivi o localizzati. Ad esempio, la categoria ambiente software viene visualizzata come categoria **SWEnv** .                         |
|    /category    |                                                                     avvia le informazioni di sistema con la categoria specificata selezionata. Usare **/showcategories** per visualizzare un elenco di ID di categoria disponibili.                                                                     |
|   /Categories   |                          avvia le informazioni di sistema solo con la categoria o le categorie specificate. Limita inoltre l'output alla categoria o alle categorie selezionate. Usare **/showcategories** per visualizzare un elenco di ID di categoria disponibili.                          |
|       /?        |                                                                                                                     Visualizza la guida al prompt dei comandi.                                                                                                                     |

## <a name="remarks"></a>Note
Alcune categorie di informazioni di sistema contengono grandi quantità di dati. È possibile utilizzare il comando **Avvia/Wait** per ottimizzare le prestazioni di Reporting per queste categorie. Per ulteriori informazioni, vedere [informazioni di sistema](https://technet.microsoft.com/library/cc783305(v=ws.10).aspx).
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
Per elencare gli ID di categoria disponibili, digitare:
```
msinfo32 /showcategories
```
Per avviare lo strumento informazioni di sistema con tutte le informazioni disponibili visualizzate, ad eccezione dei moduli caricati, digitare:
```
msinfo32 /categories +all -loadedmodules
```
Per visualizzare solo le informazioni di riepilogo sistema e creare un file con estensione NFO denominato syssum. NFO che contiene informazioni nella categoria Riepilogo sistema, digitare:
```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```
Per visualizzare le informazioni sui conflitti di risorse e creare un file con estensione NFO denominato Conflicts. NFO che contiene informazioni sui conflitti di risorse, digitare:
```
msinfo32 /nfo conflicts.nfo /categories    +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```
## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

