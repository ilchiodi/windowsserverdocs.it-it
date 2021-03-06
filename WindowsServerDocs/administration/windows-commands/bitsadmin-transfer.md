---
title: bitsadmin transfer
description: Argomento di riferimento per il comando Bitsadmin Transfer, che trasferisce uno o più file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c7011f3ef3e85d7453e63d9a9c2e4a89a52cddf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707754"
---
# <a name="bitsadmin-transfer"></a>bitsadmin transfer

Trasferisce uno o più file. Per impostazione predefinita, il servizio BITSAdmin crea un processo di download che viene eseguito con priorità **normale** e aggiorna la finestra di comando con le informazioni sullo stato di avanzamento fino al completamento del trasferimento o fino a quando non si verifica un errore critico.

Il servizio di completamento del processo se correttamente trasferisce tutti i file e Annulla il processo si verifica un errore critico. Il servizio non crea il processo se non è in grado di aggiungere file al processo o se si specifica un valore non valido per il *tipo* o *job_priority*. Per trasferire più di un file, specificare più `<RemoteFileName>-<LocalFileName>` coppie. Le coppie devono essere delimitate da spazi.

> [!NOTE]
> Il comando BITSAdmin continua a funzionare se si verifica un errore temporaneo. Per terminare il comando, premere CTRL + C.

## <a name="syntax"></a>Sintassi

```
bitsadmin /transfer <name> [<type>] [/priority <job_priority>] [/ACLflags <flags>] [/DYNAMIC] <remotefilename> <localfilename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| name | Nome del processo. Questo comando non può essere un GUID. |
| type | Facoltativa. Imposta il tipo di processo, tra cui:<ul><li>**Scaricare.** Valore predefinito. Scegliere questo tipo per i processi di download.</li><li>**/UPLOAD.** Scegliere questo tipo per i processi di caricamento.</li></ul> |
| priority | Facoltativa. Imposta la priorità del processo, tra cui:<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |
| ACLflags | Facoltativa. Indica che si desidera mantenere il proprietario e le informazioni ACL con il file da scaricare. Specificare uno o più valori, tra cui:<ul><li>**o** -copia le informazioni sul proprietario con il file.</li><li>**g** -copia le informazioni sul gruppo con il file.</li><li>**d** : copia le informazioni dell'elenco di controllo di accesso discrezionale (DACL) con file.</li><li>**s** : copia le informazioni dell'elenco di controllo di accesso di sistema (SACL) con file.</li></ul> |
| /DYNAMIC | Configura il processo utilizzando [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](https://docs.microsoft.com/windows/win32/api/bits5_0/ne-bits5_0-bits_job_property_id), che consente di ridurre i requisiti del lato server. |
| remotefilename | Nome del file dopo che è stato trasferito al server. |
| LocalFileName | Il nome del file che si trova in locale. |

## <a name="examples"></a>Esempi

Per avviare un processo di trasferimento denominato *myDownloadJob*:

```
bitsadmin /transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
