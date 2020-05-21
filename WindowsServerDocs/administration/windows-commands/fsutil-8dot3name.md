---
title: fsutil 8dot3name
description: Argomento di riferimento per il comando fsutil 8dot3name, che consente di eseguire query o modificare le impostazioni per il comportamento nome breve (8dot3 Name).
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 02977a33c21560fd2078f0f596f312f4ab4bc408
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436056"
---
# <a name="fsutil-8dot3name"></a>fsutil 8dot3name

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Esegue una query o modifica le impostazioni per il comportamento nome breve (8dot3), che include:

- Esecuzione di query sull'impostazione corrente per il comportamento del nome breve.

- Analisi del percorso di directory specificato per le chiavi del registro di sistema che potrebbero essere interessate se i nomi brevi sono stati rimossi dal percorso di directory specificato.

- Modifica dell'impostazione che controlla il comportamento del nome breve. Questa impostazione può essere applicata a un volume specificato o all'impostazione predefinita del volume.

- Rimozione dei nomi brevi per tutti i file all'interno di una directory.

> [!IMPORTANT]
> La rimozione definitiva dei nomi di file 8dot3 e la mancata modifica delle chiavi del registro di sistema che puntano ai nomi di file 8dot3 possono causare errori imprevisti dell'applicazione, inclusa l'impossibilità di disinstallare un'applicazione. Prima di provare a rimuovere i nomi di file 8dot3, è consigliabile eseguire il backup della directory o del volume.

## <a name="syntax"></a>Sintassi

```
fsutil 8dot3name [query] [<volumepath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <directorypath>
fsutil 8dot3name [set] { <defaultvalue> | <volumepath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <directorypath>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| query`[<volumepath>]` | Esegue una query sul file system per lo stato del comportamento di creazione del nome breve 8dot3.<p>Se *VolumePath* non è specificato come parametro, viene visualizzata l'impostazione predefinita del comportamento di creazione 8dot3name per tutti i volumi. |
| scansione`<directorypath>` | Analizza i file che si trovano nel *DirectoryPath* specificato per le chiavi del registro di sistema che potrebbero essere interessate se i nomi brevi di 8dot3 sono stati rimossi dai nomi file. |
| set`<defaultvalue> | <volumepath>}` | Modifica il comportamento file system per la creazione del nome 8dot3 nelle istanze seguenti:<ul><li>Quando viene specificato *DefaultValue* , la chiave del registro di sistema **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**è impostata su *DefaultValue*.<p>Il valore *DefaultValue* può includere i valori seguenti:<ul><li>**0**: Abilita la creazione del nome 8dot3 per tutti i volumi nel sistema.</li><li>**1**: Disabilita la creazione del nome 8dot3 per tutti i volumi nel sistema.</li><li>**2**: imposta la creazione del nome 8dot3 in base al volume.</li><li>**3**: Disabilita la creazione del nome 8dot3 per tutti i volumi ad eccezione del volume di sistema.</li></ul><li>Quando si specifica un *VolumePath* , le proprietà 8dot3name del flag del disco specificate sono impostate per abilitare la creazione del nome 8dot3 per un volume specificato (**0**) o per disabilitare la creazione del nome 8dot3 nel volume specificato (**1**).<p>Per abilitare o disabilitare la creazione di un nome di 8dot3 per un volume specificato, è necessario impostare il comportamento predefinito file system per la creazione del nome 8dot3 sul valore **2** .</li></ul> |
| striscia`<directorypath>` | Rimuove i nomi file 8dot3 per tutti i file che si trovano nel *DirectoryPath*specificato. Il nome del file 8dot3 non viene rimosso per i file in cui il *DirectoryPath* combinato con il nome del file contiene più di 260 caratteri.<p>Questo comando elenca, ma non modifica le chiavi del registro di sistema che puntano ai file con nomi di file 8dot3 rimossi in modo permanente. |
| `<volumepath>` | Specifica il nome dell'unità seguito da due punti o dal GUID nel formato `volume{GUID}` . |
| /f | Specifica che tutti i file che si trovano nel *DirectoryPath* specificato hanno i nomi di file 8dot3 rimossi anche se sono presenti chiavi del registro di sistema che puntano a file con il nome del file 8dot3. In questo caso, l'operazione rimuove i nomi file 8dot3, ma non modifica le chiavi del registro di sistema che puntano ai file che usano i nomi file 8dot3. **Avviso:** È consigliabile eseguire il backup della directory o del volume prima di usare il parametro **/f** perché potrebbe causare errori imprevisti dell'applicazione, inclusa l'impossibilità di disinstallare i programmi. |
| /l`[<log file>]` | Specifica un file di log in cui vengono scritte le informazioni.<p>Se il parametro **/l** non è specificato, tutte le informazioni vengono scritte nel file di log predefinito: `%temp%\8dot3_removal_log@(GMT YYYY-MM-DD HH-MM-SS)` . log * * |
| /s | Specifica che l'operazione deve essere applicata alle sottodirectory del *DirectoryPath*specificato. |
| /t | Specifica che la rimozione dei nomi di file 8dot3 deve essere eseguita in modalità test. Vengono eseguite tutte le operazioni eccetto la rimozione effettiva dei nomi file 8dot3. È possibile usare la modalità di test per individuare le chiavi del registro di sistema che puntano a file che usano i nomi file 8dot3. |
| /v | Specifica che tutte le informazioni scritte nel file di log vengono visualizzate anche nella riga di comando. |

### <a name="examples"></a>Esempi

Per eseguire una query per disabilitare il comportamento del nome 8dot3 per un volume del disco specificato con il GUID, {928842df-5A01-11de-a85c-806e6f6e6963}, digitare:

```
fsutil 8dot3name query volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

È anche possibile eseguire una query sul comportamento del nome 8dot3 usando il sottocomando **Behavior** .

Per rimuovere i nomi file 8dot3 nella directory *D:\MyData* e in tutte le sottodirectory, mentre si scrivono le informazioni nel file di log specificato come *logfile. log*, digitare:

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [fsutil behavior](fsutil-behavior.md)
