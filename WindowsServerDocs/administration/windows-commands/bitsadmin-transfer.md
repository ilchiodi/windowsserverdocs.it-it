---
title: bitsadmin Transfer
description: Windows Commands Topic for Bitsadmin Transfer, che trasferisce uno o più file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e960b4d94416d57e6c42ec27057dafef5e44516
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849004"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Trasferisce uno o più file. Per trasferire più file, specificare più \<RemoteFileName\>-\<LocalFileName\> coppie. Le coppie sono delimitati da spazi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Name|Il nome del processo. Diversamente dalla maggior parte dei comandi, il **nome** può essere solo un nome e non un GUID.|
|Type|Facoltativo: specificare il tipo di processo. Usare **/download** (impostazione predefinita) per un processo di download o **tra/upload** per un processo di caricamento.|
|Priorità|Facoltativo: impostare il job_priority su uno dei valori seguenti:</br>-IN PRIMO PIANO</br>-ALTO</br>-NORMALE</br>-BASSO|
|ACLFlags|Facoltativo: indica che si desidera mantenere il proprietario e le informazioni ACL con il file da scaricare. Ad esempio, per mantenere il proprietario e il gruppo con il file, impostare i flag su `OG`. Specificare uno o più dei flag seguenti:</br>-O: Copiare informazioni sul proprietario con file.</br>-G: Copiare le informazioni di gruppo con file.</br>-D: Copiare informazioni DACL con file.</br>-S: Copiare informazioni SACL con file.|
|/DYNAMIC|Configura il processo con [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), che attenua i requisiti del lato server.|
|RemoteFileName|Nome del file quando viene trasferito al server.|
|LocalFileName|Il nome del file che si trova in locale.|

## <a name="remarks"></a>Note

Per impostazione predefinita, il servizio BITSAdmin crea un processo di download che viene eseguito con **NORMALE** priorità e aggiorna la finestra di comando con informazioni sullo stato fino a quando il trasferimento è stato completato o finché non si verifica un errore critico. Il servizio di completamento del processo se correttamente trasferisce tutti i file e Annulla il processo si verifica un errore critico. Il servizio non crea il processo se non è possibile aggiungere file al processo o se si specifica un valore valido per *tipo* o *Job_Priority*. Per trasferire più file, specificare più *RemoteFileName*-*LocalFileName* coppie. Le coppie sono delimitati da spazi.

> [!NOTE]
> Il comando BITSAdmin continua a funzionare se si verifica un errore temporaneo. Per terminare il comando, premere CTRL + C.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

L'avvio di esempio seguente il trasferimento del processo con denominato *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)