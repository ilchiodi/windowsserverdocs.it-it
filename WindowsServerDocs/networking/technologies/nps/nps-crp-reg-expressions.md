---
title: Usare espressioni regolari nel Server dei criteri di rete
description: Questo argomento viene illustrato l'utilizzo delle espressioni regolari per criteri di ricerca in Criteri di rete in Windows Server 2016. È possibile usare questa sintassi per specificare le condizioni di attributi di criteri di rete e le aree di autenticazione RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f51bfb1c767c0eee3aed64df9879dd0a97f2f7b1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446158"
---
# <a name="use-regular-expressions-in-nps"></a>Usare espressioni regolari nel Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento viene illustrato l'utilizzo delle espressioni regolari per criteri di ricerca in Criteri di rete in Windows Server 2016. È possibile usare questa sintassi per specificare le condizioni di attributi di criteri di rete e le aree di autenticazione RADIUS.

## <a name="pattern-matching-reference"></a>Riferimento criteri di ricerca

È possibile utilizzare la tabella seguente come un'origine di riferimento durante la creazione di espressioni regolari con caratteri jolly.


|  Carattere  |                                                                                 Descrizione                                                                                  |                                                                 Esempio                                                                 |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
|     `\`     |                                                              Contrassegna il carattere successivo come un carattere in modo che corrispondano.                                                               |                      `/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`                       |
|     `^`     |                                                                 Corrisponde all'inizio dell'input o della riga.                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    Corrisponde alla fine dell'input o della riga.                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             Corrisponde al carattere precedente zero o più volte.                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              Corrisponde al carattere precedente una o più volte.                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              Corrisponde a volte il carattere zero o uno precedente.                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           Corrisponde a qualsiasi carattere singolo ad eccezione di un carattere di nuova riga.                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         Corrisponde a "criterio" e memorizza la corrispondenza.<br />In modo che corrispondano ai caratteri letterali `(` e `)` (parentesi), usare `\(` o `\)`.                         |                                                                 &nbsp;                                                                  |
|     \`x     |                                                                                     y \`                                                                                     |                                                         Corrisponde a x o y.                                                          |
|   `{n} `    |                                                          Trova la corrispondenza esatta n volte \(n è non\-numero intero negativo\).                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          Trova la corrispondenza almeno n volte \(n è non\-numero intero negativo\).                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                Trova la corrispondenza almeno n e al massimo m volte \(m e n siano non\-numeri interi negativi\).                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       Corrisponde a uno qualsiasi dei caratteri racchiusi \(un set di caratteri\).                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  Corrisponde a qualsiasi carattere che non sono racchiusi \(un set di caratteri negativi\).                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              Corrisponde a un confine di parola \(ad esempio, uno spazio\).                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         Corrisponde a un limite non alfabetico.                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       Corrisponde a un carattere di cifra \(equivalenti a cifre da 0 a 9\).                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           Corrisponde a un carattere non numerico \(equivalente a `[^0-9]` \).                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        Corrisponde a un carattere di avanzamento.                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        Corrisponde a una riga di carattere di avanzamento.                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     Corrisponde a un carattere di ritorno a capo.                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   Corrisponde a qualsiasi carattere di spazio vuoto incluso lo spazio, scheda e avanzamento carta \(equivalente a `[ \f\n\r\t\v]` \).                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  Corrisponde a qualsiasi carattere diverso da spazi vuoti \(equivalente a `[^ \f\n\r\t\v]` \).                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           Corrisponde a un carattere di tabulazione.                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      Corrisponde a un carattere di tabulazione verticale.                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              Corrisponde a qualsiasi carattere alfanumerico, incluso un carattere di sottolineatura \(equivalente a `[A-Za-z0-9_]` \).                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           Corrisponde a qualsiasi diverso da\-carattere alfanumerico, esclusione di un carattere di sottolineatura \(equivale a `[^A-Za-z0-9_]` \).                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | Si riferisce alle corrispondenze memorizzate \( `?num`, dove Bloc num è un numero intero positivo\).  Questa opzione può essere utilizzata solo nel **sostituire** casella di testo quando si configura la manipolazione degli attributi. |                                       `\1` sostituisce quello presente nella prima corrispondenza memorizzata.                                       |
|   `/n/ `    |                      Consente l'inserimento dei codici ASCII all'interno di espressioni regolari \( `?n`, dove n è un valore di escape ottali, esadecimali o decimali\).                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>Esempi per gli attributi di criteri di rete

Negli esempi seguenti viene descritto l'utilizzo della sintassi di corrispondenza dei modelli per specificare gli attributi di criteri di rete:

- Per specificare tutti i numeri di telefono all'interno di indicativo di località 899, la sintassi è:

     `899.*`

- Per specificare un intervallo di indirizzi IP che iniziano con 192.168.1, la sintassi è:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Esempi per la modifica del nome dell'area di autenticazione nell'attributo nome utente

Negli esempi seguenti viene descritto l'utilizzo della sintassi per modificare i nomi dell'area di autenticazione per l'attributo nome utente, che si trova in corrispondenza di **attributo** scheda nelle proprietà di un criterio di richiesta di connessione.

**Per rimuovere la parte dell'area di autenticazione dell'attributo nome utente**

In uno scenario di connessione remota in outsourcing in cui un Internet service provider \(ISP\) instrada le richieste di connessione a un'organizzazione dei criteri di rete, il proxy RADIUS ISP potrebbe richiedere un nome dell'area di autenticazione per instradare la richiesta di autenticazione. Tuttavia, i criteri di rete potrebbero non riconoscere la parte del nome dell'area di autenticazione del nome utente. Pertanto, il nome dell'area di autenticazione deve essere rimosso dal proxy RADIUS ISP prima che venga inoltrata a criteri di rete dell'organizzazione.

- Trova: @microsoft \.com

- Sostituisci:

**Per sostituire <em>user@example.microsoft.com</em> con *example.microsoft.com\user***

- Trovare:`(.*)@(.*)`

- Sostituire:`$2\$1`



**Per sostituire *dominio\utente* con *specific_domain\user***

- Trovare:`(.*)\\(.*)`

- Sostituisci: *dominio_specifico*`\$2`



<strong>Per sostituire *utente* con *user@specific_domain</strong>*

- Trovare:`$`

- Sostituisci: @*dominio_specifico*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Esempio per l'inoltro dei messaggi da un server proxy RADIUS

È possibile creare le regole di routing che inoltrano i messaggi RADIUS con un nome specificato dell'area di autenticazione a un set di server RADIUS quando NPS viene usato come proxy RADIUS. Di seguito è una sintassi consigliata per il routing delle richieste basata sul nome dell'area di autenticazione.

- **Nome NetBIOS**: `WCOAST`
- **Modello**:      `^wcoast\\`

Nell'esempio seguente, wcoast.microsoft.com è un suffisso di nome principale (UPN) utente univoco per il wcoast.microsoft.com di dominio Active Directory o DNS. Usando il modello fornito, il proxy NPS possibile instradare i messaggi in base al nome NetBIOS del dominio o il suffisso UPN.

- **Nome NetBIOS**: `WCOAST`
- **Suffisso UPN**:   `wcoast.microsoft.com`
- **Modello**:      `^wcoast\\|@wcoast\.microsoft\.com$`


Per altre informazioni sulla gestione dei criteri di rete, vedere [gestire i Server dei criteri di rete](nps-manage-top.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
