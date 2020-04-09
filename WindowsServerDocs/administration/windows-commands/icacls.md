---
title: icacls
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 59d10b9ed681b7e0af120798dde9f200182d67d3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842264"
---
# <a name="icacls"></a>icacls

Visualizza o modifica gli elenchi di controllo di accesso discrezionali (DACL) nei file specificati e applica gli elenchi DACL archiviati ai file nelle directory specificate.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<FileName >|Specifica il file per cui visualizzare gli elenchi DACL.|
|> Directory \<|Specifica la directory per cui visualizzare gli elenchi DACL.|
|/t|Esegue l'operazione su tutti i file specificati nella directory corrente e nelle relative sottodirectory.|
|/c|L'operazione continuerà nonostante eventuali errori nel file. Verranno ancora visualizzati messaggi di errore.|
|/l|Esegue l'operazione su un collegamento simbolico e la relativa destinazione.|
|/q|Elimina i messaggi di esito positivo.|
|[/Save \<ACLfile > [/t] [/c] [/l] [/q]]|Archivia gli elenchi DACL per tutti i file corrispondenti in *ACLfile* per un utilizzo successivo con **/ripristino**.|
|[/SetOwner \<username > [/t] [/c] [/l] [/q]]|Modifica il proprietario di tutti i file corrispondenti all'utente specificato.|
|[/findSID \<SID > [/t] [/c] [/l] [/q]]|Trova tutti i file corrispondenti che contengono un DACL citare in modo esplicito l'identificatore specificato di sicurezza (SID).|
|[/ verificare [/t] [/c] [/l] [/q]]|Trova tutti i file con gli ACL non canonico o hanno lunghezze non coerente con i conteggi ACE (voce di controllo di accesso).|
|[/Reset [/t] [/c] [/l] [/q]]|Sostituisce gli ACL predefinito ereditata ACL per tutti i file corrispondenti.|
|[/dy [: r] \<SID >:<Perm>[...]]|Concede specificato diritti di accesso utente. Autorizzazioni di sostituiscono le autorizzazioni precedentemente concesse esplicite.</br>Senza **: r**, le autorizzazioni vengono aggiunte a qualsiasi precedentemente concesse le autorizzazioni esplicite.|
|[/Deny \<SID >:<Perm>[...]]|Nega in modo esplicito i diritti di accesso utente specificato. L'impostazione esplicita Nega ACE viene aggiunto per le autorizzazioni indicate e vengono rimosse le stesse autorizzazioni in qualsiasi autorizzazione esplicita.|
|[/Remove [: g\|:d]] \<SID > [...]] /t /c /l /q|Rimuove tutte le occorrenze del SID specificato dall'elenco DACL.</br>**: g** Rimuove tutte le occorrenze di concessione dei diritti per il SID specificato.</br>**: d** Rimuove tutte le occorrenze dei diritti negato l'accesso al SID specificato.|
|[/setintegritylevel [(CI) (OI)] livello\<>:<Policy>[...]]|Aggiunge una voce ACE di integrità in modo esplicito per tutti i file corrispondenti. *Livello* viene specificato come:</br>-   **L**[ow]</br>-   **M**[edium]</br>-   **H**[IGH]</br>Opzioni di ereditarietà per l'integrità ACE possono precedere il livello e vengono applicate solo alla directory.|
|[/substitute \<SidOld > <SidNew> [...]]|Sostituisce un SID esistente (*SidOld*) con un nuovo SID (*SidNew*). Richiede il *Directory* parametro.|
|/Restore \<ACLfile > [/c] [/l] [/q]|Si applica stored DACL da *ACLfile* ai file nella directory specificata. Richiede il *Directory* parametro.|
|/InheritanceLevel: [e\|d\|r]|Imposta il livello di ereditarietà: <br>  **e** -Abilita enheritance <br>**d** : Disabilita l'ereditarietà e copia le voci ACE <br>**r** -rimuove tutte le voci ACE ereditate

## <a name="remarks"></a>Note

-   SID potrebbero essere in qualsiasi formato di nome descrittivo o numerici. Se si usa un formato numerico, applicare il carattere **&#42;** jolly all'inizio del SID.
-   **Icacls** mantiene l'ordine canonico delle voci ACE come:  
    -   Negazioni esplicite
    -   Concede l'esplicita
    -   Rifiuti ereditati
    -   Concede ereditati
-   *Perm* è una maschera di autorizzazione che può essere specificata in uno dei seguenti formati:  
    -   Una sequenza di diritti di base:

        **F** (accesso completo)

        **M** (modifica dell'accesso)

        **RX** (lettura ed esecuzione)

        **R** (accesso in sola lettura)

        **W** (accesso in sola scrittura)
    -   Un elenco delimitato da virgole tra parentesi di diritti specifici:

        **D** (eliminare)

        **RC** (controllo lettura)

        **WDAC** (scrittura DAC)

        **WO** (proprietario scrittura)

        **S** (sincronizzazione)

        **AS** (accesso di sicurezza del sistema)

        **MA** (impostazione massima consentita)

        **GR** (lettura generica)

        **GW** (scrittura generica)

        **GE** (esecuzione generica)

        **GA** (generico tutti)

        **Desktop remoto** (lettura directory/elenco di dati)

        **WD** (scrittura file/Aggiungi dati)

        **Active Directory** (aggiungere/aggiunta dati sottodirectory)

        **REA** (lettura attributi estesi)

        **WEA** (scrittura attributi estesi)

        **X** (eseguire/incrociato)

        **Controller di dominio** (eliminare figlio)

        **RA** (lettura attributi)

        **WA** (scrivere attributi)
-   Diritti di ereditarietà possono precedere uno *Perm* form essi vengono applicati solo alle directory:

    **(OI)** : l'oggetto eredita

    **(CI)** : eredità

    **(I/o)** : solo eredità

    **(NP)** : non vengono propagate ereditano

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
