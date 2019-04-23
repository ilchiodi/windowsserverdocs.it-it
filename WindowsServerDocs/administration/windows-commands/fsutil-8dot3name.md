---
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
title: Fsutil 8dot3name
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4e8d67de42342f5a0828df9f57ca6d7e2870586e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873192"
---
# <a name="fsutil-8dot3name"></a>Fsutil 8dot3name

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Esegue una query o modifica le impostazioni per il comportamento di breve nome (8.3), tra cui:

-   L'impostazione corrente per il comportamento di nome breve di query

-   Analizza il percorso della directory specificato per le chiavi del Registro di sistema che potrebbero essere interessate se i nomi brevi sono stati rimossi dal percorso di directory specificato

-   Modificare l'impostazione che controlla il comportamento di nome breve. Questa impostazione può essere applicata a un volume specificato o per l'impostazione del volume predefinita.

-   Rimuovere i nomi brevi per tutti i file contenuti in una directory

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil 8dot3name [query] [<VolumePath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <DirectoryPath>
fsutil 8dot3name [set] { <DefaultValue> | <VolumePath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <DirectoryPath>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|query [<VolumePath>]|Esegue una query nel file system per lo stato del comportamento di creazione del nome breve 8.3.<br /><br />Se un *VolumePath* non è specificato come un parametro, viene visualizzata l'impostazione del comportamento predefinito 8dot3name creazione per tutti i volumi.|
|scan <DirectoryPath>|Analizza i file che si trovano nell'oggetto specificato *DirectoryPath* per le chiavi del Registro di sistema che potrebbero essere interessate se i nomi brevi 8.3 sono stati rimossi dai nomi di file.|
|set { <DefaultValue> &#124; <VolumePath>}|Modifica il comportamento del file system per la creazione di nomi 8.3 nei casi seguenti:<br /><br /><ul><li>Quando *DefaultValue* è specificato, la chiave del Registro di sistema, **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**, è impostato il *DefaultValue*.<br /><br />    Il *DefaultValue* può avere i valori seguenti:<br /><br /><ul><li>**0**: Consente la creazione di nomi 8.3 per tutti i volumi del sistema.</li><li>**1**: Disabilita la creazione di nomi 8.3 per tutti i volumi del sistema.</li><li>**2**: Imposta la creazione di nomi 8.3 in base al volume.</li><li>**3**: Disabilita la creazione di nomi 8.3 per tutti i volumi, ad eccezione del volume di sistema.</li></ul></li><li>Quando un *VolumePath* è specificato, i volumi specificati in proprietà del disco flag 8dot3name sono impostati per consentire la creazione di nomi 8.3 per un volume specificato (**0**) o un set per disabilitare la creazione di nomi 8.3 nel specificato un volume (**1**).<br /><br />    È necessario impostare il comportamento predefinito del file system per la creazione di nomi 8.3 sul valore **2** prima di abilitare o disabilitare la creazione di nomi 8.3 per un volume specificato.</li></ul>|
|strip <DirectoryPath>|Rimuove i nomi di file 8.3 per tutti i file che si trovano nell'oggetto specificato *DirectoryPath*. Il nome di file 8.3 non viene rimosso per tutti i file in cui il *DirectoryPath* combinato con il file il nome contiene più di 260 caratteri.<br /><br />Questo comando Elenca, ma non modifica le chiavi del Registro di sistema che puntano ai file con nomi di file 8.3 rimossi in modo permanente.<br /><br />Per altre informazioni sugli effetti della rimozione in modo permanente i nomi di file 8.3 dai file, vedere [osservazioni](Fsutil-8dot3name.md#BKMK_remarks).|
|<VolumePath>|Specifica il nome di unità seguito da una virgola o il GUID nel formato **Volume {***GUID***}**.|
|/f|Specifica che tutti i file che si trovano nell'oggetto specificato *DirectoryPath* sono i nomi di file 8.3 rimossi anche se sono presenti chiavi del Registro di sistema che puntano ai file usando il nome di file 8.3. In questo caso, l'operazione rimuove i nomi di file 8.3, ma non modifica le chiavi del Registro di sistema che puntano ai file che utilizzano i nomi di file 8.3. **Avviso:** È consigliabile eseguire il backup della directory o un volume prima di usare la **/f** parametro perché potrebbe generare errori imprevisti delle applicazioni, tra cui l'impossibilità di disinstallare i programmi.|
|/l [<log file>]|Specifica un file di log in cui le informazioni vengono scritte.<br /><br />Se il **/l** parametro non viene specificato, tutte le informazioni vengono scritte nel file di log predefinito:<br /><br />**% temp%\8dot3_removal_log@ (***GMT AAAA-MM-GG HH-MM-SS***). log**|
|/s|Specifica che l'operazione deve essere applicato alle sottodirectory dell'oggetto specificato *DirectoryPath*.|
|/t|Specifica che la rimozione dei nomi di file 8.3 deve essere eseguita in modalità di test. Vengono eseguite tutte le operazioni tranne l'effettiva rimozione dei nomi di file 8.3. È possibile utilizzare la modalità di test per individuare quali chiavi di puntano ai file che utilizzano i nomi di file 8.3 del Registro di sistema.|
|/v|Specifica che tutte le informazioni che viene scritto nel file di log viene visualizzate anche nella riga di comando.|

## <a name="BKMK_remarks"></a>Sezione Osservazioni

-   Rimuovere definitivamente i nomi di file 8.3 ed evitare di modificare le chiavi del Registro di sistema che fanno riferimento ai nomi di file 8.3 potrebbe causare errori imprevisti delle applicazioni, inclusa l'impossibilità per disinstallare un'applicazione. È consigliabile che innanzitutto eseguire il backup della directory o un volume prima di provare a rimuovere i nomi di file 8.3.

## <a name="BKMK_examples"></a>Esempi
Per eseguire una query per il comportamento di nome 8.3 disable per un volume del disco che viene specificato con il GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, digitare:

```
fsutil 8dot3name query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

È anche possibile eseguire una query il comportamento di nomi 8.3 usando il **comportamento** sottocomando.

Per rimuovere i nomi di file 8.3 nel *D:\MyData* directory e tutte le sottodirectory, durante la scrittura di informazioni nel file di log che viene specificato come *MyLogFile*, tipo:

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)

[Comportamento fsutil](Fsutil-behavior.md)


