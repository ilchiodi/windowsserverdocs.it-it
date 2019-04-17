---
title: L'abilitazione del banner di individuazione di estensione
description: L'abilitazione del banner di individuazione di estensione
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/08/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 389acba6d1fe6f65320bd780c9fde6469b387e0b
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262968"
---
# L'abilitazione del banner di individuazione di estensione #

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Una nuova funzionalità disponibile in Windows Admin Center Preview 1903 è il banner di individuazione di estensione. Questa funzionalità consente a un'estensione dichiarare il produttore dell'hardware server e supporta i modelli e quando un utente si connette a un server o cluster per cui è disponibile un'estensione, verrà visualizzato un banner di notifica per installare facilmente l'estensione. Gli sviluppatori di estensione sarà in grado di ottenere una maggiore visibilità per le estensioni e gli utenti saranno in grado di individuare facilmente altre funzionalità di gestione per i server.

![Banner di individuazione di estensione](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## Come funziona il banner di individuazione di estensione ##

Quando Windows Admin Center viene avviata, verrà connettersi ai feed di estensione registrati e recuperare i metadati per i pacchetti di estensioni disponibili. Quindi quando un utente si connette a un server o cluster in Windows Admin Center, Microsoft legge il produttore dell'hardware server e il modello per visualizzare lo strumento di panoramica. Se ti trovi un'estensione che dichiara di supportare produttore e/o modello del server corrente, verrà visualizzato un banner per far sapere all'utente. Facendo clic sul collegamento "Configurare ora" verrà visualizzato all'utente di Gestione estensioni in cui possono installare l'estensione.

## Come implementare il banner di individuazione di estensione ##

I metadati "tag" nel file .nuspec viene usato per dichiarare il produttore dell'hardware e/o modelli supportati dall'estensione. I tag sono delimitati da spazi e puoi aggiungere un produttore o tag modello o entrambi per dichiarare il produttore supportati e/o modelli. Formato del tag ``"[value type]_[value condition]"`` dove [tipo di valore] è il "Produttore" o "Modello" (maiuscole/minuscole) e [condizione valore] è un' [espressione regolare Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) che definisce il produttore o una stringa di modello e [tipo di valore] e [value condizione] vengono separati da un carattere di sottolineatura. Questa stringa viene quindi codificata tramite URI codifica e aggiunto alla stringa di metadati .nuspec "tag".

### Esempio ###

Si supponga che ho sviluppato un'estensione che supporta i server da una società denominata Contoso Inc., con il modello di nome R3xx e R4xx.

1. Il tag per il produttore sarebbe ``"Manufacturer_/Contoso Inc./"``. Potrebbe essere il tag per i modelli di ``"Model_/^R[34][0-9]{2}$/"``. A seconda di come strettamente vuoi definire la condizione di corrispondenza, saranno presenti diversi modi per definire l'espressione regolare. È anche possibile separare i tag produttore o il modello in più tag, ad esempio, potrebbe essere anche il tag di modello ``"Model_/R3../ Model_/R4../"``.
2. Puoi testare l'espressione regolare con Console relativa agli strumenti del browser web. In Edge o Chrome, premere F12 per aprire la finestra relativa agli strumenti e nella scheda Console, digita l'invio di hit e seguente:

```javascript
var regex = /^R[34][0-9]{2}$/
```

Quindi se puoi digitare ed eseguire il comando seguente, verrà restituito "true".

```javascript
regex.test('R300')
```

E se Esegui il comando seguente, verrà restituito "false".

```javascript
regex.test('R500')
```

3. Dopo aver verificato dell'espressione regolare, è possibile codificarlo nella Console di relativa agli strumenti, utilizzando il metodo Javascript seguente:

```javascript
encodeURI(/^R[34][0-9]{2}$/)
```

Il formato finale della stringa di tag da aggiungere al file .nuspec sarebbe:

```
<tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
```

> [!Tip]
> Ci rendiamo che un produttore dell'hardware può avere un numero considerevole di cui alcune potrebbero essere supportate mentre alcuni non sono i nomi di modello. Tieni presente che questa funzionalità è progettata per semplificare l' **individuazione** di estensione, ma non deve essere un inventario di tutti i modelli di perfettamente aggiornati. Puoi definire l'espressione regolare per essere un'espressione più semplice che corrisponde a un sottoinsieme dei modelli. Un utente potrebbe non vedere il banner di individuazione se prima si connettono a un modello di server che non soddisfano la condizione, ma prima o poi i si connettono a un altro server che e verrà individuare e installare l'estensione. Anche considerare la definizione di un'espressione regolare semplice che corrisponde solo il nome del produttore. In alcuni casi, l'estensione in realtà potrebbe non supportare un modello specifico, ma è possibile usare [strumento dinamico visualizzare delle funzionalità](./dynamic-tool-display.md) per definire uno script di PowerShell personalizzato per controllare il supporto del modello e mostrare solo l'estensione quando applicabili o fornire limitata funzionalità nell'estensione per i modelli che non supportano tutte le funzionalità.