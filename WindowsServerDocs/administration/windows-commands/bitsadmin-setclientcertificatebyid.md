---
title: bitsadmin setclientcertificatebyid
description: Argomento di riferimento per il comando Bitsadmin setclientcertificatebyid, che specifica l'identificatore del certificato client da utilizzare per l'autenticazione client in una richiesta HTTPS (SSL)
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24f5d0b9cda9fecc70611d8eaa21b0c8976c4c7c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719336"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

Specifica l'identificatore del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL).

## <a name="syntax"></a>Sintassi

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| store_location | Identifica la posizione di un archivio di sistema da utilizzare per la ricerca del certificato, tra cui:<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>Servizi</li><li>UTENTI</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE.</li></ul> |
| store_name | Nome dell'archivio certificati, tra cui:<ul><li>CA (certificati dell'autorità di certificazione)</li><li>MY (certificati personali)</li><li>RADICE (certificati radice)</li><li>SPC (certificato autore del software).</li></ul> |
| hexadecimal_cert_id | Numero esadecimale che rappresenta l'hash del certificato. |

## <a name="examples"></a>Esempi

Per specificare l'identificatore del certificato client da usare per l'autenticazione client in una richiesta HTTPS (SSL) per il processo denominato *myDownloadJob*:

```
bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
