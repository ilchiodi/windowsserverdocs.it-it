---
title: nslookup ls
description: Argomento di riferimento per il comando nslookup ls, che elenca le informazioni sul dominio DNS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87e30ed5d5b44b805c3b3b004feb5ed252b5a760
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721124"
---
# <a name="nslookup-ls"></a>nslookup ls

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elenca le informazioni sul dominio DNS.

## <a name="syntax"></a>Sintassi

```
ls [<option>] <DNSdomain> [{[>] <filename>|[>>] <filename>}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<option>` | Le opzioni valide includono:<ul><li>**-t:** Elenca tutti i record del tipo specificato. Per ulteriori informazioni, vedere [nslookup set querytype](nslookup-set-querytype.md).</li><li>**-a:** Elenca gli alias dei computer nel dominio DNS. Questo parametro è uguale a **-t CNAME**</li><li>**-d:** Elenca tutti i record per il dominio DNS. Questo parametro è uguale a **-t any**</li><li>**-h:** Elenca le informazioni sulla CPU e sul sistema operativo per il dominio DNS. Questo parametro è uguale a **-t HINFO**</li><li>**-s:** Elenca i servizi noti dei computer nel dominio DNS. Questo parametro è uguale a **-t WKS**. |
| `<DNSdomain>` | Specifica il dominio DNS per cui si desidera ottenere informazioni. |
| `<filename>` | Specifica un nome file da utilizzare per l'output salvato. È possibile utilizzare i caratteri maggiore di ( `>` ) e Double maggiore di ( `>>` ) per reindirizzare l'output nel modo consueto. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- L'output predefinito di questo comando include i nomi dei computer e i relativi indirizzi IP associati.

- Se l'output viene indirizzato a un file, vengono aggiunti i contrassegni hash per ogni 50 record ricevuti dal server.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup set querytype](nslookup-set-querytype.md)
