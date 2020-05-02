---
title: icacls
description: Argomento di riferimento per il comando icacls, che Visualizza o modifica gli elenchi di controllo di accesso discrezionale (DACL) sui file specificati e applica i DACL archiviati ai file nelle directory specificate.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: dcf4fa9fa9205a762ead99ac4a8486ac04c23514
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724869"
---
# <a name="icacls"></a>icacls

Visualizza o modifica gli elenchi di controllo di accesso discrezionali (DACL) nei file specificati e applica gli elenchi DACL archiviati ai file nelle directory specificate.

> [!NOTE]
> Questo comando sostituisce il [comando cacls](cacls.md)deprecato.

## <a name="syntax"></a>Sintassi

```
icacls <filename> [/grant[:r] <sid>:<perm>[...]] [/deny <sid>:<perm>[...]] [/remove[:g|:d]] <sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<policy>[...]]
icacls <directory> [/substitute <sidold> <sidnew> [...]] [/restore <aclfile> [/c] [/l] [/q]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<filename>` | Specifica il file per cui visualizzare gli elenchi DACL. |
| `<directory>` | Specifica la directory per cui visualizzare gli elenchi DACL. |
| /t | Esegue l'operazione su tutti i file specificati nella directory corrente e nelle relative sottodirectory. |
| /C | L'operazione continuerà nonostante eventuali errori nel file. Verranno ancora visualizzati messaggi di errore. |
| /l | Esegue l'operazione su un collegamento simbolico anziché sulla relativa destinazione. |
| /q | Elimina i messaggi di esito positivo. |
| [/ Salva `<ACLfile>` [/t] / [c] [/l] [/q]] | Archivia gli elenchi DACL per tutti i file corrispondenti in *ACLfile* per un utilizzo successivo con **/ripristino**. |
| [/ setowner `<username>` [/t] / [c] [/l] [/q]] | Modifica il proprietario di tutti i file corrispondenti all'utente specificato. |
| [/findsid `<sid>` [/t] [/c] [/l] [/q]] | Trova tutti i file corrispondenti che contengono un DACL citare in modo esplicito l'identificatore specificato di sicurezza (SID). |
| [/ verificare [/t] [/c] [/l] [/q]] | Trova tutti i file con gli ACL non canonico o hanno lunghezze non coerente con i conteggi ACE (voce di controllo di accesso). |
| [/Reset [/t] [/c] [/l] [/q]] | Sostituisce gli ACL predefinito ereditata ACL per tutti i file corrispondenti. |
| [/dy [: r] \<SID>:<perm>[...]] | Concede specificato diritti di accesso utente. Autorizzazioni di sostituiscono le autorizzazioni precedentemente concesse esplicite.<p>Se non si aggiunge **: r**, le autorizzazioni vengono aggiunte a qualsiasi autorizzazione esplicita precedentemente concessa. |
| [/Deny \<SID>:<perm>[...]] | Nega in modo esplicito i diritti di accesso utente specificato. L'impostazione esplicita Nega ACE viene aggiunto per le autorizzazioni indicate e vengono rimosse le stesse autorizzazioni in qualsiasi autorizzazione esplicita. |
| [/Remove`[:g | :d]]` `<sid>`[...] /t /c /l /q | Rimuove tutte le occorrenze del SID specificato dall'elenco DACL. Questo comando può anche usare:<ul><li>**: g** -rimuove tutte le occorrenze dei diritti concessi al SID specificato.</li><li>**:d** : rimuove tutte le occorrenze dei diritti negati al SID specificato. |
| [/setintegritylevel [(CI) (OI)] `<Level>:<Policy>`[...]] | Aggiunge una voce ACE di integrità in modo esplicito per tutti i file corrispondenti. Il livello può essere specificato come:<ul><li>**l** -basso</li><li>**m**-medio</li><li>**h** -alto</li></ul>Opzioni di ereditarietà per l'integrità ACE possono precedere il livello e vengono applicate solo alla directory. |
| [/ sostituire `<sidold> <sidnew>` [...]] | Sostituisce un SID esistente (*sidold*) con un nuovo SID (*sidnew*). Richiede l'utilizzo di `<directory>` con il parametro. |
| / ripristino `<ACLfile>` [/c] [/l] [/q] | Applica gli elenchi DACL archiviati `<ACLfile>` da ai file nella directory specificata. Richiede l'utilizzo di `<directory>` con il parametro. |
| InheritanceLevel`[e | d | r]` | Imposta il livello di ereditarietà, che può essere:<ul><li>**e** -Abilita l'ereditarietà</li><li>**d** : Disabilita l'ereditarietà e copia le voci ACE</li><li>**r** -rimuove tutte le voci ACE ereditate</li></ul> |

## <a name="remarks"></a>Osservazioni

- SID potrebbero essere in qualsiasi formato di nome descrittivo o numerici. Se si usa un formato numerico, applicare il carattere jolly **&#42;** all'inizio del SID.

- Questo comando conserva l'ordine canonico delle voci ACE come segue:  

    - Negazioni esplicite

    -  Concede l'esplicita

    - Rifiuti ereditati

    - Concede ereditati

- L' `<perm>` opzione è una maschera di autorizzazione che può essere specificata in uno dei formati seguenti:

    - Una sequenza di diritti di base:

      - **F** -accesso completo

      - **M**-modificare l'accesso

      - **RX** -lettura ed esecuzione dell'accesso

      - **R** -accesso in sola lettura

      - **W** Accesso con accesso in sola scrittura

    - Un elenco delimitato da virgole tra parentesi di diritti specifici:

      - **D** -eliminazione

      - Controllo di lettura **RC**

      - **WDAC** -scrittura DAC

      - Proprietario **wo** -Write

      - **S** -Sincronizza

      - Sicurezza del sistema **As** -Access

      - **Ma** -massimo consentito

      - **Gr** -lettura generica

      - **GW** -scrittura generica

      - **GE** -esecuzione generica

      - **GA** -generico all

      - Directory di dati/elenco di lettura **Desktop remoto**

      - **WD** -scrivere dati/Aggiungi file

      - **Ad** -Accoda dati/Aggiungi sottodirectory

      - **Rea** -lettura degli attributi estesi

      - **WEA** -scrivere attributi estesi

      - **X** -esecuzione/attraversamento

      - **DC** -Elimina figlio

      - Attributi di lettura **ra**

      - Attributi **WA** -Write

  - I diritti di ereditarietà possono precedere uno dei due `<perm>` moduli e vengono applicati solo alle directory:

      - **(OI)** -l'oggetto eredita

      - **(Ci)** -contenitore eredita

      - **(Io)** -solo ereditarietà

      - **(NP)** -non propagare eredita

## <a name="examples"></a>Esempi

Per salvare il DACL per tutti i file nella directory C:\Windows e nelle relative sottodirectory al file ACLFile, digitare:

```
icacls c:\windows\* /save aclfile /t
```

Per ripristinare il DACL per ogni file all'interno di ACLFile presente nella directory C:\Windows e nelle relative sottodirectory, digitare:

```
icacls c:\windows\ /restore aclfile
```

Per concedere all'utente User1 le autorizzazioni di eliminazione e scrittura dell'applicazione livello dati in un file denominato Test1, digitare:

```
icacls test1 /grant User1:(d,wdac)
```

Per concedere all'utente definito da SID S-1-1-0 le autorizzazioni per l'eliminazione e la scrittura dell'applicazione livello dati in un file, denominato Test2, digitare:

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
