---
title: choice
description: Windows Commands Topic for choice, che richiede all'utente di selezionare un elemento da un elenco di scelte a carattere singolo in un programma batch, quindi restituisce l'indice della scelta selezionata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26f915bc35b9edf3b206ff65e011b7f4a22988b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847804"
---
# <a name="choice"></a>choice

Richiede all'utente di selezionare un elemento da un elenco di scelte a carattere singolo in un file batch e quindi restituisce l'indice dell'opzione selezionata. Se utilizzata senza parametri, **scelta** consente di visualizzare le scelte predefinite **Y** e **N**.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <Text>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/c \<Choice1 ><Choice2><... >|Specifica l'elenco di scelte da creare. Le scelte valide includono-z, A-Z, 0-9 e caratteri ASCII estesi (128-254). L'elenco predefinito è YN, che viene visualizzato come `[Y,N]?`.|
|/n|Nasconde l'elenco di scelte, anche se sono ancora abilitati le scelte disponibili e il testo del messaggio (se specificato da **/m**) è ancora visualizzato.|
|/cs|Specifica che le opzioni di maiuscole e minuscole. Per impostazione predefinita, le scelte non sono rilevanti.|
|/t \<timeout >|Specifica il numero di secondi di pausa prima di utilizzare l'opzione predefinita specificata da **/d**. I valori accettabili sono compresi **0** a **9999**. Se **/t** è impostato su **0**, **scelta** non sospendere prima di restituire la scelta predefinita.|
|/d \<Choice >|Specifica la scelta predefinita da utilizzare dopo un'attesa il numero di secondi specificato da **/t**. La scelta predefinita deve essere nell'elenco di scelte specificato da **/c**.|
|<Text>/m|Specifica un messaggio da visualizzare prima dell'elenco di scelte. Se **/m** non è specificato, viene visualizzato solo il messaggio desiderato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   La variabile di ambiente ERRORLEVEL è impostata per l'indice della chiave a cui l'utente seleziona dall'elenco di scelte. La prima opzione nell'elenco restituisce un valore pari a 1, il secondo è un valore di 2 e così via. Se l'utente preme un tasto che non è una scelta valida, **scelta** emette un segnale acustico di avviso. Se **scelta** rileva una condizione di errore viene restituito un valore ERRORLEVEL di 255. Se l'utente preme CTRL + INTERR o CTRL + C, **scelta** restituisce un valore ERRORLEVEL pari a 0.

> [!NOTE]
> Quando si utilizzano valori ERRORLEVEL in un file batch, elencarli in ordine decrescente.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per presentare le proprie scelte Y, N, C, digitare la riga seguente in un file batch:
```
choice /c ync
```
Il seguente messaggio viene visualizzato quando il file batch viene eseguito il **scelta** comando:
```
[Y,N,C]?
```
Per nascondere le scelte Y, N e C, ma visualizzare il testo Sì, no o continua, digitare la riga seguente in un file batch:
```
choice /c ync /n /m Yes, No, or Continue?
```
Il seguente messaggio viene visualizzato quando il file batch viene eseguito il **scelta** comando:
```
Yes, No, or Continue?
```

> [!NOTE]
> Se si utilizza il **/n** parametro, ma non si utilizza **/m**, l'utente non è richiesto quando **scelta** è in attesa di input.

Per visualizzare testo e le opzioni utilizzate negli esempi precedenti, digitare la riga seguente in un file batch:
```
choice /c ync /m Yes, No, or Continue
```
Il seguente messaggio viene visualizzato quando il file batch viene eseguito il **scelta** comando:
```
Yes, No, or Continue [Y,N,C]?
```
Per impostare un limite di cinque secondi e specificare **N** come valore predefinito, digitare la riga seguente in un file batch:
```
choice /c ync /t 5 /d n
```
Il seguente messaggio viene visualizzato quando il file batch viene eseguito il **scelta** comando:
```
[Y,N,C]?
```

> [!NOTE]
> In questo esempio, se l'utente non preme un tasto entro cinque secondi, **scelta** Seleziona **N** per impostazione predefinita e restituisce un valore di errore pari a 2. In caso contrario, **scelta** restituisce il valore corrispondente alla scelta dell'utente.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)