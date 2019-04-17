---
title: Usare le espressioni regolari in Criteri di rete
description: Questo argomento illustra l'uso delle espressioni regolari di corrispondenze in Criteri di rete in Windows Server 2016. È possibile utilizzare questa sintassi per specificare le condizioni di attributi di criteri di rete e le aree di autenticazione RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f84173f1f51be9fd44995dc41f759bbea4fb3539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-regular-expressions-in-nps"></a>Usare le espressioni regolari in Criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento illustra l'uso delle espressioni regolari di corrispondenze in Criteri di rete in Windows Server 2016. È possibile utilizzare questa sintassi per specificare le condizioni di attributi di criteri di rete e le aree di autenticazione RADIUS.

## <a name="pattern-matching-reference"></a>Corrispondenza di riferimento

È possibile utilizzare la tabella seguente come origine riferimento durante la creazione di espressioni regolari con caratteri jolly.

|Carattere|Descrizione|Esempio|
|---------|-----------|-------|
|`\`  |Contrassegna il carattere successivo come carattere per trovare la corrispondenza. |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |Corrisponde all'inizio di una riga o di input. | &nbsp; |
|`$`  |Corrisponde alla fine di una riga o di input. | &nbsp; |
|`*`  |Corrisponde al carattere precedente zero o più volte. |`/zo*/ matches either "z" or "zoo."` |
|`+`  |Corrisponde al carattere precedente una o più volte. |`/zo+/ matches "zoo" but not "z."` |
|`?`  |Corrispondenze precedente caratteri o meno uno. |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |Corrisponde a qualsiasi carattere singolo ad eccezione di un carattere di nuova riga.  | &nbsp; |
|`( pattern )`  |Corrisponde a "modello" e memorizza la corrispondenza.   |`To match ( ) (parentheses), use "\(" or "\)".`  |
|' x | Y '  |Corrisponde a x e y.  |' /z|Food? / corrisponde a "zoo" o "food". ` |
|`{ n } `  |Esattamente n corrispondenze \ (n è un integer\ negativo).  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{ n ,}`  |Almeno n corrispondenze \ (n è un integer\ negativo).  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{ n , m }`  |Corrispondenze almeno n e al massimo m \ (m e n sono integers\ negativo).  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[ xyz ]`  |Corrisponde a uno qualsiasi dei caratteri racchiusi \(a character set\).  |`/[abc]/ matches the "a" in "plain."`  |
|`[^ xyz ]`  |Corrisponde a tutti i caratteri non sono racchiuse \ (un set di caratteri negativo).  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |Corrisponde a un limite di word \ (ad esempio, space\).  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |Corrisponde a una parola.  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |Corrisponde a una cifra \ (equivalente a cifre da 0 a 9 \).  | &nbsp; |
|`\D`  |Corrisponde a un carattere diverso da cifra \ (equivalente a `[^0-9]`\).  | &nbsp; |
|`\f`  |Corrispondenze un carattere di avanzamento.  | &nbsp; |
|`\n`  |Corrispondenze avanzamento riga.  | &nbsp; |
|`\r`  |Corrisponde a un ritorno a capo.  | &nbsp; |
|`\s`  |Corrisponde a qualsiasi carattere spazio vuoto tra spazio, scheda e avanzamento modulo \ (equivalente a `[ \f\n\r\t\v]`\).  | &nbsp; |
|`\S`  |Corrisponde a qualsiasi carattere diverso dallo spazio \ (equivalente a `[^ \f\n\r\t\v]`\).  | &nbsp; |
|`\t`  |Corrisponde a un carattere di tabulazione.  | &nbsp; |
|`\v`  |Corrisponde a un carattere tabulazione verticale.  | &nbsp; |
|`\w`  |Corrisponde a qualsiasi carattere, tra cui carattere di sottolineatura \ (equivalente a `[A-Za-z0-9_]`\).  | &nbsp; |
|`\W`  |Corrisponde a qualsiasi carattere word, l'esclusione di carattere di sottolineatura \ (equivalente a `[^A-Za-z0-9_]`\).  | &nbsp; |
|`\ num`  |Si riferisce a corrispondenze ricordate \ (`?num`, dove num è un integer\ positivi).  Questa opzione può essere utilizzata solo nel **sostituire** casella di testo quando si configura la manipolazione degli attributi.| `\1` Sostituisce quello presente nella prima corrispondenza da ricordare.  |
|`/ n / `  |Consente l'inserimento di codici ASCII nelle espressioni regolari \ (`?n`, dove n è un value\ escape ottale, decimale o esadecimale).  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>Esempi per gli attributi dei criteri di rete

Gli esempi seguenti viene descritto l'utilizzo di caratteri jolly per specificare gli attributi dei criteri di rete:

- Per specificare i numeri di telefono all'interno di indicativo di località 899, la sintassi è:

     `899.*`

- Per specificare un intervallo di indirizzi IP che iniziano con 192.168.1, la sintassi è:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Esempi per la modifica del nome dell'area di autenticazione nell'attributo di nome utente

Gli esempi seguenti viene descritto l'utilizzo della sintassi per modificare i nomi dell'area di autenticazione per l'attributo di nome utente, che si trova in corrispondenza di **attributo** scheda delle proprietà di un criterio di richiesta di connessione.

**Per rimuovere la parte dell'area di autenticazione dell'attributo del nome utente**

In uno scenario di connessione remota in outsourcing, in cui una Internet service provider \(ISP\) route richieste di connessione a un server dei criteri di rete dell'organizzazione, il proxy RADIUS ISP potrebbe richiedere un nome dell'area di autenticazione per indirizzare la richiesta di autenticazione. Tuttavia, il server dei criteri di rete potrebbe non riconoscere la parte del nome dell'area di autenticazione del nome utente. Di conseguenza, il nome dell'area di autenticazione deve essere rimossa dal proxy RADIUS ISP prima che venga inoltrato al server dei criteri di rete dell'organizzazione.

- Trovare:@microsoft\.com

- Sostituire:

**Per sostituire *user@example.microsoft.com*con *example.microsoft.com\user n.***

- Trovare:`(.*)@(.*)`

- Sostituire:`$2\$1`



**Per sostituire *dominio\utente* con *specific_domain\user***

- Trovare:`(.*)\\(.*)`

- Sostituisci: *dominio_specifico*`\$2`



**Per sostituire *utente* con*user@specific_domain***

- Trovare:`$`

- Sostituisci: @*dominio_specifico*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Esempio di inoltro dei messaggi da un server proxy RADIUS

È possibile creare regole di routing per l'inoltro di messaggi RADIUS con un nome dell'area di autenticazione specificato a un set di server RADIUS quando criteri di rete viene usato come proxy RADIUS. Di seguito è una sintassi consigliata per il routing delle richieste in base al nome dell'area di autenticazione.

- **Nome NetBIOS **: `WCOAST`
- **Modello **:      `^wcoast\\`

Nell'esempio seguente, wcoast.microsoft.com è un suffisso di nome dell'entità (UPN) utente univoco per il dominio di Active Directory o DNS wcoast.microsoft.com. Utilizzando il modello fornito, il proxy di criteri di rete può instradare messaggi in base al nome NetBIOS del dominio o il suffisso UPN.

- **Nome NetBIOS **: `WCOAST`
- **Suffisso UPN **:   `wcoast.microsoft.com`
- **Modello **:      `^wcoast\\|@wcoast\.microsoft\.com$`


Per ulteriori informazioni sulla gestione dei criteri di rete, vedere [gestire Server dei criteri di rete](nps-manage-top.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
