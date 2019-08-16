---
title: Usare espressioni regolari nel Server dei criteri di rete
description: In questo argomento viene illustrato l'utilizzo di espressioni regolari per la corrispondenza dei criteri in NPS in Windows Server 2016. È possibile usare questa sintassi per specificare le condizioni degli attributi dei criteri di rete e delle aree di autenticazione RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fec546e0608c36f9b3d907e486a0a3a24e7d1728
ms.sourcegitcommit: e04565e4a1fb7aaed04addd2bc87cc6ec4c82e81
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529904"
---
# <a name="use-regular-expressions-in-nps"></a>Usare espressioni regolari nel Server dei criteri di rete

>Si applica a Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene illustrato l'utilizzo di espressioni regolari per la corrispondenza dei criteri in NPS in Windows Server 2016. È possibile usare questa sintassi per specificare le condizioni degli attributi dei criteri di rete e delle aree di autenticazione RADIUS.

## <a name="pattern-matching-reference"></a>Riferimento ai criteri di ricerca

È possibile usare la tabella seguente come origine di riferimento durante la creazione di espressioni regolari con la sintassi dei criteri di ricerca.


|  Carattere  |                                                                                 Descrizione                                                                                  |                                                                 Esempio                                                                 |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
|     `\`|                                                              Contrassegna il carattere successivo come carattere da confrontare.                                                               |                      `/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`                       |
|     `^`     |                                                                 Corrisponde all'inizio dell'input o della riga.                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    Corrisponde alla fine dell'input o della riga.                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             Trova la corrispondenza del carattere precedente zero o più volte.                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              Trova la corrispondenza del carattere precedente una o più volte.                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              Trova la corrispondenza del carattere precedente zero o una volta.                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           Corrisponde a qualsiasi carattere singolo eccetto un carattere di nuova riga.                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         Corrisponde a "pattern" e ricorda la corrispondenza.<br />Per trovare la corrispondenza con `(` i `)` caratteri letterali e (parentesi `\(` ) `\)`, usare o.                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               Corrisponde a x o y.                                                          |
|   `{n} `    |                                                          Corrisponde esattamente a n \(volte n è un\-numero intero\)non negativo.                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          Corrisponde a almeno n volte \(n è un numero\-intero\)non negativo.                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                Corrisponde a almeno n e al massimo m volte \(m e n sono numeri\-interi\)non negativi.                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       Corrisponde a uno qualsiasi dei caratteri \(racchiusi in un set\)di caratteri.                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  Corrisponde a qualsiasi carattere non incluso \(in un set\)di caratteri negativi.                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              Corrisponde a un confine \(di parola, ad esempio\)uno spazio.                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         Corrisponde a un confine non alfanumerico.                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       Corrisponde a un carattere \(di cifra equivalente a cifre comprese tra\)0 e 9.                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           Corrisponde a un carattere \(non numerico equivalente a. `[^0-9]` \)                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        Corrisponde a un carattere di avanzamento del modulo.                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        Corrisponde a un carattere di avanzamento riga.                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     Corrisponde a un carattere di ritorno a capo.                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   Corrisponde a qualsiasi carattere di spazio vuoto, inclusi spazio, tabulazione \(e feed `[ \f\n\r\t\v]`di form equivalente a \).                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  Corrisponde a qualsiasi carattere \(diverso da uno spazio vuoto equivalente a. `[^ \f\n\r\t\v]` \)                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           Corrisponde a un carattere di tabulazione.                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      Corrisponde a un carattere di tabulazione verticale.                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              Corrisponde a qualsiasi carattere alfanumerico, incluso \(il carattere `[A-Za-z0-9_]`di sottolineatura equivalente a \).                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           Corrisponde a qualsiasi\-carattere non alfanumerico, escluso il `[^A-Za-z0-9_]`carattere di sottolineatura \(equivalente a \).                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | Si riferisce \(alle corrispondenze `?num`memorizzate, dove\)num è un numero intero positivo.  Questa opzione può essere utilizzata solo nella casella di testo Sostituisci quando si configura la manipolazione degli attributi. |                                       `\1`sostituisce quello archiviato nella prima corrispondenza memorizzata.                                       |
|   `/n/ `    |                      Consente l'inserimento di codici ASCII in espressioni \( `?n`regolari, dove n è un valore\)di escape ottale, esadecimale o decimale.                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>Esempi per gli attributi dei criteri di rete

Negli esempi seguenti viene descritto l'uso della sintassi di corrispondenza dei modelli per specificare gli attributi dei criteri di rete:

- Per specificare tutti i numeri di telefono nel codice area 899, la sintassi è:

     `899.*`

- Per specificare un intervallo di indirizzi IP che iniziano con 192.168.1, la sintassi è:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Esempi per la modifica del nome dell'area di autenticazione nell'attributo nome utente

Negli esempi seguenti viene descritto l'uso della sintassi di corrispondenza dei modelli per modificare i nomi dell'area di autenticazione per l'attributo nome utente, che si trova nella scheda **attributo** delle proprietà di un criterio di richiesta di connessione.

**Per rimuovere la parte dell'area di autenticazione dell'attributo del nome utente**

In uno scenario di connessione remota esternalizzato in cui un provider \(\) di servizi Internet invia le richieste di connessione a un server dei criteri di rete, il proxy RADIUS ISP potrebbe richiedere un nome dell'area di autenticazione per instradare la richiesta di autenticazione. Tuttavia, il server dei criteri di dominio potrebbe non riconoscere la parte del nome dell'area di autenticazione del nome utente. Pertanto, il nome dell'area di autenticazione deve essere rimosso dal proxy RADIUS del provider di servizi Internet prima che venga inviato all'organizzazione NPS.

- Trova: @microsoft \.com

- Sostituisci:

**Per sostituire <em>user@example.microsoft.com</em> con _example. Microsoft. com\utente_**

- Trovare`(.*)@(.*)`

- Sostituire`$2\$1`



**Per sostituire _dominio\utente_ con _specific_domain\user_**

- Trovare`(.*)\\(.*)`

- Sostituisci: *specific_domain*`\$2`



<strong>Per sostituire l' *utente* con *user@specific_domain</strong>*

- Trovare`$`

- Sostituisci: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Esempio per l'inoltro di messaggi RADIUS da un server proxy

È possibile creare regole di routing che inoltrino i messaggi RADIUS con un nome dell'area di autenticazione specificato a un set di server RADIUS quando NPS viene usato come proxy RADIUS. Di seguito è riportata una sintassi consigliata per il routing di richieste basate sul nome dell'area di autenticazione.

- **Nome NetBIOS**:`WCOAST`
- **Modello**:`^wcoast\\`

Nell'esempio seguente wcoast.microsoft.com è un suffisso del nome dell'entità utente (UPN) univoco per il dominio DNS o Active Directory wcoast.microsoft.com. Utilizzando il modello fornito, il proxy NPS può instradare i messaggi in base al nome NetBIOS del dominio o al suffisso UPN.

- **Nome NetBIOS**:`WCOAST`
- **Suffisso UPN**:`wcoast.microsoft.com`
- **Modello**:`^wcoast\\|@wcoast\.microsoft\.com$`


Per ulteriori informazioni sulla gestione dei server dei criteri di rete, vedere [Manage Network Policy Server](nps-manage-top.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
