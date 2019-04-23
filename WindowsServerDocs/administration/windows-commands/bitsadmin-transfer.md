---
title: bitsadmin Transfer
description: Argomento i comandi di Windows per **trasferimento bitsadmin** -trasferisce uno o più file.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ef29a242a834fae42d1de3994a82aedcf87ec2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852462"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Trasferisce uno o più file. Per trasferire più file, specificare più \<RemoteFileName\>-\<LocalFileName\> coppie. Le coppie sono delimitati da spazi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Nome|Il nome del processo. La maggior parte dei comandi, a differenza delle **nome** può essere solo un nome e non è un GUID.|
|Tipo|Facoltativo: specificare il tipo di processo. Uso **/scaricare** (predefinito) per un processo di download oppure **/caricare** per un processo di caricamento.|
|Priority|Facoltativo: impostare il job_priority a uno dei valori seguenti:</br>-IN PRIMO PIANO</br>-ALTO</br>-NORMALE</br>-BASSO|
|ACLFlags|Facoltativo: indica che si vuole mantenere il proprietario e le informazioni di ACL con il file in corso il download. Ad esempio, per mantenere il proprietario e il gruppo con il file, i flag di `OG`. Specificare uno o più dei flag seguenti:</br>-   O: Copiare informazioni sul proprietario con file.</br>-   G: Copiare le informazioni sui gruppi con file.</br>-   D: Copiare informazioni DACL con file.</br>-   S: Copiare informazioni SACL con file.|
|\/DYNAMIC|Consente di configurare il processo con [ **BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), che riduce i requisiti lato server.|
|RemoteFileName|Nome del file quando viene trasferito al server.|
|LocalFileName|Il nome del file che si trova in locale.|

## <a name="remarks"></a>Note

Per impostazione predefinita, il servizio BITSAdmin crea un processo di download che viene eseguito con **NORMALE** priorità e aggiorna la finestra di comando con informazioni sullo stato fino a quando il trasferimento è stato completato o finché non si verifica un errore critico. Il servizio di completamento del processo se correttamente trasferisce tutti i file e Annulla il processo si verifica un errore critico. Il servizio non crea il processo se non è possibile aggiungere file al processo o se si specifica un valore valido per *tipo* o *Job_Priority*. Per trasferire più file, specificare più *RemoteFileName*-*LocalFileName* coppie. Le coppie sono delimitati da spazi.

> [!NOTE]
> Il comando BITSAdmin continua a funzionare se si verifica un errore temporaneo. Per terminare il comando, premere CTRL + C.

## <a name="BKMK_examples"></a>Esempi

L'avvio di esempio seguente il trasferimento del processo con denominato *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)