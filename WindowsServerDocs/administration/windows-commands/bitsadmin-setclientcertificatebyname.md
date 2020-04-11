---
title: setclientcertificatebyname Bitsadmin
description: Windows Commands Topic for **BITSAdmin setclientcertificatebyname**, che specifica il nome del soggetto del certificato client da usare per l'autenticazione client in una richiesta HTTPS (SSL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2308bb5331f1555965b278a64bb7ab95e03779b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123051"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>setclientcertificatebyname Bitsadmin

Specifica il nome del soggetto del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL).

## <a name="syntax"></a>Sintassi

```
bitsadmin /setclientcertificatebyname <job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| store_location | Identifica la posizione di un archivio di sistema da utilizzare per la ricerca del certificato. I valori possibili sono:<ul><li>1 (CURRENT_USER)</li><li>2 (LOCAL_MACHINE)</li><li>3 (CURRENT_SERVICE)</li><li>4 (SERVIZI)</li><li>5 (UTENTI)</li><li>6 (CURRENT_USER_GROUP_POLICY)</li><li>7 (LOCAL_MACHINE_GROUP_POLICY)</li><li>8 (LOCAL_MACHINE_ENTERPRISE)</li></ul> |
| store_name | Nome dell'archivio certificati. I valori possibili sono:<ul><li>CA (certificati dell'autorità di certificazione)</li><li>MY (certificati personali)</li><li>RADICE (certificati radice)</li><li>SPC (certificato autore del software)</li></ul> |
| subject_name | Nome del certificato. |

## <a name="examples"></a>Esempi

Nell'esempio seguente viene specificato il nome *del certificato client da utilizzare* per l'autenticazione client in una richiesta HTTPS (SSL) per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /setclientcertificatebyname myDownloadJob 1 MY myCertificate
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)