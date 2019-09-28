---
title: bitsadmin Transfer
description: Argomento dei comandi di Windows per **BITSAdmin Transfer** -trasferisce uno o più file.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2a12e6e2023c979d5b0c095c1eddd77eb5155d1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380348"
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
|Nome|Il nome del processo. Diversamente dalla maggior parte dei comandi, il **nome** può essere solo un nome e non un GUID.|
|Type|Facoltativo: specificare il tipo di processo. Usare **/download** (impostazione predefinita) per un processo di download o **tra/upload** per un processo di caricamento.|
|Priority|Facoltativo: impostare job_priority su uno dei valori seguenti:</br>-IN PRIMO PIANO</br>-ALTO</br>-NORMALE</br>-BASSO|
|ACLFlags|Facoltativo: indica che si desidera mantenere il proprietario e le informazioni ACL con il file da scaricare. Ad esempio, per mantenere il proprietario e il gruppo con il file, impostare i flag su `OG`. Specificare uno o più dei flag seguenti:</br>O Copiare le informazioni sul proprietario con il file.</br>G Copia le informazioni sul gruppo con il file.</br>D Copiare le informazioni DACL con file.</br>S Copiare le informazioni SACL con il file.|
|\/DYNAMIC|Configura il processo con [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), che attenua i requisiti del lato server.|
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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)