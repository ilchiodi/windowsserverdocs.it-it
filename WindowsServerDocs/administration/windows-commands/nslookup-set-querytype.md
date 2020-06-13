---
title: nslookup set querytype
description: Argomento di riferimento per il comando nslookup set querytype, che consente di modificare il tipo di record di risorse per la query.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c54671d23fb7fd9500ba7aac1d59cf50fef78ead
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721607"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il tipo di record di risorse per la query. Per informazioni sui tipi di record di risorse, vedere [Request for Comment (RFC) 1035](https://tools.ietf.org/html/rfc1035).

> [!NOTE]
> Questo comando è lo stesso del comando [nslookup set Type](nslookup-set-type.md) .

## <a name="syntax"></a>Sintassi

```
set querytype=<resourcerecordtype>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<resourcerecordtype>` | Specifica un tipo di record di risorse DNS. Il tipo di record **di**risorse predefinito è, ma è possibile usare uno dei valori seguenti:<ul><li>**R:** Specifica l'indirizzo IP di un computer.</li><li>**Qualsiasi:** Specifica l'indirizzo IP di un computer.</li><li>**CNAME:** Specifica un nome canonico per un alias.</li><li>**GID** Specifica un identificatore di gruppo di un nome di gruppo.</li><li>**HINFO:** Specifica la CPU e il tipo di sistema operativo di un computer.</li><li>**MB:** Specifica un nome di dominio della cassetta postale.</li><li>**Mg:** Specifica un membro del gruppo di posta elettronica.</li><li>**MInfo:** Specifica le informazioni sull'elenco di cassette postali o di posta</li><li>**Mr:** Specifica il nome di dominio per la ridenominazione della posta.</li><li>**MX:** Specifica lo scambiatore di posta elettronica.</li><li>**NS:** Specifica un server dei nomi DNS per la zona denominata.</li><li>**Ptr:** Specifica un nome di computer se la query è un indirizzo IP; in caso contrario, specifica il puntatore ad altre informazioni.</li><li>**SOA:** Specifica l'inizio dell'autorità per una zona DNS.</li><li>**Txt:** Specifica le informazioni di testo.</li><li>**UID:** Specifica l'identificatore utente.</li><li>**UINFO:** Specifica le informazioni utente.</li><li>**WKS:** Descrive un servizio noto.</li></ul> |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup set type](nslookup-set-type.md)
