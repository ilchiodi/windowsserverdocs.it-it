---
title: setclientcertificatebyname Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin setclientcertificatebyname** -specifica il nome del soggetto del certificato client da usare per l'autenticazione client in una richiesta HTTPS (SSL).
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de2e84401673848ecc8823bb6dd3f91224d9a87e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380675"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>setclientcertificatebyname Bitsadmin



Specifica il nome del soggetto del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL).

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Store_location|Identifica la posizione di un archivio di sistema da utilizzare per la ricerca del certificato. I valori possibili sono:</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVIZI)</br>5 (UTENTI)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Il nome dell'archivio certificati. I valori possibili sono:</br>CA (certificati dell'autorità di certificazione)</br>MY (certificati personali)</br>RADICE (certificati radice)</br>SPC (certificato autore del software)|
|Subject_name|Nome del certificato|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene specificato il nome *del certificato client da utilizzare* per l'autenticazione client in una richiesta HTTPS (SSL) per il processo denominato *il mio lavoro*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)