---
title: setclientcertificatebyid Bitsadmin
description: Windows Commands Topic for **BITSAdmin setclientcertificatebyid**, che specifica l'identificatore del certificato client da usare per l'autenticazione client in una richiesta HTTPS (SSL)
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 376bb850664a5ed569488634029cb7384856f158
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123041"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>setclientcertificatebyid Bitsadmin

Specifica l'identificatore del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL).

## <a name="syntax"></a>Sintassi

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| store_location | Identifica la posizione di un archivio di sistema da utilizzare per la ricerca del certificato, tra cui:<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>Servizi</li><li>UTENTI</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE.</li></ul> |
| store_name | Nome dell'archivio certificati, tra cui:<ul><li>CA (certificati dell'autorità di certificazione)</li><li>MY (certificati personali)</li><li>RADICE (certificati radice)</li><li>SPC (certificato autore del software).</li></ul> |
| hexadecimal_cert_id | Numero esadecimale che rappresenta l'hash del certificato. |

## <a name="examples"></a>Esempi

Nell'esempio seguente viene specificato l'identificatore del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL) per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)