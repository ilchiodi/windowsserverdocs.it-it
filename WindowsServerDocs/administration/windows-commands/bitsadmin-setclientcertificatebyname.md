---
title: bitsadmin setclientcertificatebyname
description: Argomento di riferimento per il comando Bitsadmin setclientcertificatebyname, che specifica il nome del soggetto del certificato client da usare per l'autenticazione client in una richiesta HTTPS (SSL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef7111d462cd0509e8959855e9bc950dc15f3bfe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719343"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname

Specifica il nome del soggetto del certificato client da utilizzare per l'autenticazione del client in una richiesta HTTPS (SSL).

## <a name="syntax"></a>Sintassi

```
bitsadmin /setclientcertificatebyname <job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| store_location | Identifica la posizione di un archivio di sistema da utilizzare per la ricerca del certificato. I valori possibili sono:<ul><li>1 (CURRENT_USER)</li><li>2 (LOCAL_MACHINE)</li><li>3 (CURRENT_SERVICE)</li><li>4 (SERVIZI)</li><li>5 (UTENTI)</li><li>6 (CURRENT_USER_GROUP_POLICY)</li><li>7 (LOCAL_MACHINE_GROUP_POLICY)</li><li>8 (LOCAL_MACHINE_ENTERPRISE)</li></ul> |
| store_name | Nome dell'archivio certificati. I valori possibili sono:<ul><li>CA (certificati dell'autorità di certificazione)</li><li>MY (certificati personali)</li><li>RADICE (certificati radice)</li><li>SPC (certificato autore del software)</li></ul> |
| subject_name | Nome del certificato. |

## <a name="examples"></a>Esempi

Per specificare il nome *del certificato client da usare* per l'autenticazione client in una richiesta HTTPS (SSL) per il processo denominato *myDownloadJob*:

```
bitsadmin /setclientcertificatebyname myDownloadJob 1 MY myCertificate
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
