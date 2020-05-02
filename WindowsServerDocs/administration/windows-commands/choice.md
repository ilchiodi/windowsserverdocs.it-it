---
title: choice
description: Argomento di riferimento per il comando Choice, che richiede all'utente di selezionare un elemento da un elenco di scelte a carattere singolo in un programma batch, quindi restituisce l'indice della scelta selezionata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32c0daa680178c1952015c62c6c6749acf5f6143
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82713539"
---
# <a name="choice"></a>choice

Richiede all'utente di selezionare un elemento da un elenco di scelte a carattere singolo in un file batch e quindi restituisce l'indice dell'opzione selezionata. Se utilizzata senza parametri, **scelta** consente di visualizzare le scelte predefinite **Y** e **N**.

## <a name="syntax"></a>Sintassi

```
choice [/c [<choice1><choice2><…>]] [/n] [/cs] [/t <timeout> /d <choice>] [/m <text>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /c`<choice1><choice2><…>` | Specifica l'elenco di scelte da creare. Le scelte valide includono-z, A-Z, 0-9 e caratteri ASCII estesi (128-254). L'elenco predefinito è YN, che viene visualizzato come `[Y,N]?`. |
| /n | Nasconde l'elenco di scelte, anche se sono ancora abilitati le scelte disponibili e il testo del messaggio (se specificato da **/m**) è ancora visualizzato. |
| /cs | Specifica che le opzioni di maiuscole e minuscole. Per impostazione predefinita, le scelte non sono rilevanti. |
| /t`<timeout>` | Specifica il numero di secondi di pausa prima di utilizzare l'opzione predefinita specificata da **/d**. I valori accettabili sono compresi **0** a **9999**. Se **/t** è impostato su **0**, **scelta** non sospendere prima di restituire la scelta predefinita. |
| /d`<choice>` | Specifica la scelta predefinita da utilizzare dopo un'attesa il numero di secondi specificato da **/t**. La scelta predefinita deve essere nell'elenco di scelte specificato da **/c**. |
| /m`<text>` | Specifica un messaggio da visualizzare prima dell'elenco di scelte. Se **/m** non è specificato, viene visualizzato solo il messaggio desiderato. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Osservazioni

- La variabile di ambiente **errorlevel** è impostata sull'indice della chiave che l'utente seleziona dall'elenco di scelte. La prima opzione nell'elenco restituisce un valore `1`, il secondo valore `2`e così via. Se l'utente preme un tasto che non è una scelta valida, **scelta** emette un segnale acustico di avviso. 

- Se **Choice** rileva una condizione di errore, restituisce un valore **errorlevel** pari `255`a. Se l'utente preme CTRL + INTERr o CTRL + C, **Choice** restituisce un valore **errorlevel** `0`.

> [!NOTE]
> Quando si utilizzano valori **errorlevel** in un programma batch, è necessario elencarli in ordine decrescente.

## <a name="examples"></a>Esempi

Per presentare le scelte **Y**, **N**e **C**, digitare la riga seguente in un file batch:

```
choice /c ync
```

Il seguente messaggio viene visualizzato quando il file batch viene eseguito il **scelta** comando:

```
[Y,N,C]?
```

Per nascondere le scelte **Y**, **n**e **C**, ma visualizzare il testo **Sì**, **No**o **continua**, digitare la riga seguente in un file batch:

```
choice /c ync /n /m Yes, No, or Continue?
```

> [!NOTE]
> Se si utilizza il **/n** parametro, ma non si utilizza **/m**, l'utente non è richiesto quando **scelta** è in attesa di input.

Per visualizzare testo e le opzioni utilizzate negli esempi precedenti, digitare la riga seguente in un file batch:

```
choice /c ync /m Yes, No, or Continue
```

Per impostare un limite di cinque secondi e specificare **N** come valore predefinito, digitare la riga seguente in un file batch:

```
choice /c ync /t 5 /d n
```

> [!NOTE]
> In questo esempio, se l'utente non preme un tasto entro cinque secondi, **Choice** seleziona **N** per impostazione predefinita e restituisce un valore di `2`errore. In caso contrario, **scelta** restituisce il valore corrispondente alla scelta dell'utente.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
