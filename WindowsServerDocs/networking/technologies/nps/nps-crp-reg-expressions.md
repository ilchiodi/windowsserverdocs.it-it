---
title: Usare espressioni regolari nel Server dei criteri di rete
description: In questo argomento viene illustrato l'utilizzo di espressioni regolari per la corrispondenza dei criteri in NPS in Windows Server. È possibile usare questa sintassi per specificare le condizioni degli attributi dei criteri di rete e delle aree di autenticazione RADIUS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: jgerend
author: jasongerend
msdate: 08/16/2019
ms.openlocfilehash: 94bb9b54dba046c57c6f82e6a52a71adbcf4ce75
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396381"
---
# <a name="use-regular-expressions-in-nps"></a>Usare espressioni regolari nel Server dei criteri di rete

> Si applica a:  Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

In questo argomento viene illustrato l'utilizzo di espressioni regolari per la corrispondenza dei criteri in NPS in Windows Server. È possibile usare questa sintassi per specificare le condizioni degli attributi dei criteri di rete e delle aree di autenticazione RADIUS.

## <a name="pattern-matching-reference"></a>Riferimento ai criteri di ricerca

È possibile usare la tabella seguente come origine di riferimento durante la creazione di espressioni regolari con la sintassi dei criteri di ricerca. Si noti che i modelli di espressione regolare sono spesso racchiusi tra barre (/).

|  Carattere  |  Descrizione  |   Esempio                                                                 |
| ----------- | ------------- | ------------------------------------------------------------------------  |
|     `\ `     | Indica che il carattere che segue è un carattere speciale o che deve essere interpretato letteralmente.  | `/n/ matches the character "n" while the sequence /\n/ matches a line feed or newline character.`  |
|     `^`     |                                                                 Corrisponde all'inizio dell'input o della riga.                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    Corrisponde alla fine dell'input o della riga.                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             Trova la corrispondenza del carattere precedente zero o più volte.                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              Trova la corrispondenza del carattere precedente una o più volte.                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              Trova la corrispondenza del carattere precedente zero o una volta.                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           Corrisponde a qualsiasi carattere singolo eccetto un carattere di nuova riga.                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         Corrisponde a "pattern" e ricorda la corrispondenza.<br />Per trovare la corrispondenza con i caratteri letterali `(` e `)` (parentesi), utilizzare `\(` o `\)`.                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               Corrisponde a x o y.                                                          |
|   `{n} `    |                                                          Corrisponde esattamente a n volte \(n è un valore diverso da @ no__t-1negative @ no__t-2.                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          Corrisponde a almeno n volte \(n è un numero intero non @ no__t-1negative @ no__t-2.                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                Corrisponde a almeno n e al massimo m volte \(m e n sono numeri interi non @ no__t-1negative @ no__t-2.                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       Corrisponde a uno qualsiasi dei caratteri racchiusi @no__t set di caratteri 0A @ no__t-1.                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  Corrisponde a qualsiasi carattere non racchiuso \(A negativo set di caratteri @ no__t-1.                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              Corrisponde a un confine di parola @no__t esempio 0per, uno spazio @ no__t-1.                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         Corrisponde a un confine non alfanumerico.                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       Corrisponde a un carattere di cifra \(equivalent a cifre comprese tra 0 e 9 @ no__t-1.                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           Corrisponde a un carattere non numerico \(equivalent a `[^0-9]` @ no__t-2.                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        Corrisponde a un carattere di avanzamento del modulo.                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        Corrisponde a un carattere di avanzamento riga.                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     Corrisponde a un carattere di ritorno a capo.                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   Corrisponde a qualsiasi carattere di spazio vuoto, incluso lo spazio, la scheda e il feed di form \(equivalent a `[ \f\n\r\t\v]` @ no__t-2.                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  Trova la corrispondenza di qualsiasi carattere diverso da uno spazio vuoto \(equivalent per `[^ \f\n\r\t\v]` @ no__t-2.                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           Corrisponde a un carattere di tabulazione.                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      Corrisponde a un carattere di tabulazione verticale.                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              Corrisponde a qualsiasi carattere alfanumerico, incluso il carattere di sottolineatura \(equivalent per `[A-Za-z0-9_]` @ no__t-2.                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           Corrisponde a qualsiasi carattere non @ no__t-0word, escluso il carattere di sottolineatura \(equivalent con `[^A-Za-z0-9_]` @ no__t-3.                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | Indica che corrisponde a \( @ no__t-1, dove num è un numero intero positivo @ no__t-2.  Questa opzione può essere utilizzata solo nella casella di testo **Sostituisci** quando si configura la manipolazione degli attributi. |                                       `\1` sostituisce quello archiviato nella prima corrispondenza memorizzata.                                       |
|   `/n/ `    |                      Consente l'inserimento di codici ASCII in espressioni regolari \( @ no__t-1, dove n è un valore di escape ottale, esadecimale o decimale @ no__t-2.                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>Esempi per gli attributi dei criteri di rete

Negli esempi seguenti viene descritto l'uso della sintassi di corrispondenza dei modelli per specificare gli attributi dei criteri di rete:

- Per specificare tutti i numeri di telefono nel codice area 899, la sintassi è:

     `899.*`

- Per specificare un intervallo di indirizzi IP che iniziano con 192.168.1, la sintassi è:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Esempi per la modifica del nome dell'area di autenticazione nell'attributo nome utente

Negli esempi seguenti viene descritto l'uso della sintassi di corrispondenza dei modelli per modificare i nomi dell'area di autenticazione per l'attributo nome utente, che si trova nella scheda **attributo** delle proprietà di un criterio di richiesta di connessione.

**Per rimuovere la parte dell'area di autenticazione dell'attributo del nome utente**

In uno scenario di connessione remota in outsourcing in cui un provider di servizi Internet \(ISP @ no__t-1 instrada le richieste di connessione a un'organizzazione NPS, il proxy RADIUS ISP potrebbe richiedere un nome di area di autenticazione per indirizzare la richiesta di autenticazione. Tuttavia, il server dei criteri di dominio potrebbe non riconoscere la parte del nome dell'area di autenticazione del nome utente. Pertanto, il nome dell'area di autenticazione deve essere rimosso dal proxy RADIUS del provider di servizi Internet prima che venga inviato all'organizzazione NPS.

- Trovare: @microsoft @ no__t-1com

- Sostituisci:

**Per sostituire <em>user@example.microsoft.com</em> con _example. Microsoft. com\utente_**

- Trova: `(.*)@(.*)`

- Sostituisci: `$2\$1`



**Per sostituire _dominio\utente_ con _specific_domain\user_**

- Trova: `(.*)\\(.*)`

- Sostituisci: *specific_domain*`\$2`



<strong>Per sostituire l' *utente* con *user@specific_domain</strong>*

- Trova: `$`

- Sostituisci: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Esempio per l'inoltro di messaggi RADIUS da un server proxy

È possibile creare regole di routing che inoltrino i messaggi RADIUS con un nome dell'area di autenticazione specificato a un set di server RADIUS quando NPS viene usato come proxy RADIUS. Di seguito è riportata una sintassi consigliata per il routing di richieste basate sul nome dell'area di autenticazione.

- **Nome NetBIOS**: `WCOAST`
- **Modello**: `^wcoast\\`

Nell'esempio seguente wcoast.microsoft.com è un suffisso del nome dell'entità utente (UPN) univoco per il dominio DNS o Active Directory wcoast.microsoft.com. Utilizzando il modello fornito, il proxy NPS può instradare i messaggi in base al nome NetBIOS del dominio o al suffisso UPN.

- **Nome NetBIOS**: `WCOAST`
- **Suffisso UPN**: `wcoast.microsoft.com`
- **Modello**: `^wcoast\\|@wcoast\.microsoft\.com$`


Per ulteriori informazioni sulla gestione dei server dei criteri di rete, vedere [Manage Network Policy Server](nps-manage-top.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
