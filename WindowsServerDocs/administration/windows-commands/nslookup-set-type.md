---
title: nslookup set type
description: Argomento di riferimento per il comando nslookup set Type, che consente di modificare il tipo di record di risorse per la query.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5248e314-fac1-413e-81dc-bbe0a0873ba5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 793ab40bbc47df09eb71642623e42a6f65c56842
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721354"
---
# <a name="nslookup-set-type"></a>nslookup set type

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il tipo di record di risorse per la query. Per informazioni sui tipi di record di risorse, vedere [Request for Comment (RFC) 1035](https://tools.ietf.org/html/rfc1035).

> [!NOTE]
> Questo comando è lo stesso del comando [nslookup set querytype](nslookup-set-querytype.md) .

## <a name="syntax"></a>Sintassi

```
set type=<resourcerecordtype>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<resourcerecordtype>` | Specifica un tipo di record di risorse DNS. Il tipo di record **di**risorse predefinito è, ma è possibile usare uno dei valori seguenti:<ul><li>**R:** Specifica l'indirizzo IP di un computer.</li><li>**Qualsiasi:** Specifica l'indirizzo IP di un computer.</li><li>**CNAME:** Specifica un nome canonico per un alias.</li><li>**GID** Specifica un identificatore di gruppo di un nome di gruppo.</li><li>**HINFO:** Specifica la CPU e il tipo di sistema operativo di un computer.</li><li>**MB:** Specifica un nome di dominio della cassetta postale.</li><li>**Mg:** Specifica un membro del gruppo di posta elettronica.</li><li>**MInfo:** Specifica le informazioni sull'elenco di cassette postali o di posta</li><li>**Mr:** Specifica il nome di dominio per la ridenominazione della posta.</li><li>**MX:** Specifica lo scambiatore di posta elettronica.</li><li>**NS:** Specifica un server dei nomi DNS per la zona denominata.</li><li>**Ptr:** Specifica un nome di computer se la query è un indirizzo IP; in caso contrario, specifica il puntatore ad altre informazioni.</li><li>**SOA:** Specifica l'inizio dell'autorità per una zona DNS.</li><li>**Txt:** Specifica le informazioni di testo.</li><li>**UID:** Specifica l'identificatore utente.</li><li>**UINFO:** Specifica le informazioni utente.</li><li>**WKS:** Descrive un servizio noto.</li></ul> |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup set type](nslookup-set-querytype.md)
