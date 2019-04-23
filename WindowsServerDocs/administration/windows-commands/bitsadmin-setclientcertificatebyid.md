---
title: setclientcertificatebyid Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin setclientcertificatebyid** specifica l'identificatore del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL)
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2424de18ee8aaec73b086207e8ef56d85df862fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863932"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>setclientcertificatebyid Bitsadmin



Specifica l'identificatore del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL).

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Store_location|Identifica la posizione di un archivio di sistema da utilizzare per la ricerca del certificato. I valori possibili sono:</br>1 (CURRENT_USER)</br>2 (COMPUTER_LOCALE)</br>3 (CURRENT_SERVICE)</br>4 (SERVIZI)</br>5 (USERS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Il nome dell'archivio certificati. I valori possibili sono:</br>Autorità di certificazione (certificati di autorità di certificazione)</br>(I certificati personali)</br>RADICE (i certificati radice)</br>SPC (certificato autori Software)|
|Hexadecimal_cert_id|Un numero esadecimale che rappresenta l'hash del certificato|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente specifica l'identificatore del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL) per il processo denominato *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)