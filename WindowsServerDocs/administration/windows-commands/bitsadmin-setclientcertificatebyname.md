---
title: setclientcertificatebyname Bitsadmin
description: Windows Commands Topic for Bitsadmin setclientcertificatebyname, che specifica il nome del soggetto del certificato client da usare per l'autenticazione client in una richiesta HTTPS (SSL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08ec6fd8c941234de36f14cd71ffa51c3b428acb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849654"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>setclientcertificatebyname Bitsadmin

Specifica il nome del soggetto del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL).

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Store_location|Identifica la posizione di un archivio di sistema da utilizzare per la ricerca del certificato. I valori possibili sono:</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVIZI)</br>5 (UTENTI)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|Nome dell'archivio certificati. I valori possibili sono:</br>CA (certificati dell'autorità di certificazione)</br>MY (certificati personali)</br>RADICE (certificati radice)</br>SPC (certificato autore del software)|
|Subject_name|Nome del certificato|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene specificato il nome *del certificato client da utilizzare* per l'autenticazione client in una richiesta HTTPS (SSL) per il processo denominato *il mio lavoro*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)