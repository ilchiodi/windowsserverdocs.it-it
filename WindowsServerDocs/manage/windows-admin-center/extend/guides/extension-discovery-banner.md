---
title: Abilitare il banner di individuazione di estensione
description: Abilitare il banner di individuazione di estensione
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fafc16d6acae3c5a7a58a379d2f88998b8e98c3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811875"
---
# <a name="enabling-the-extension-discovery-banner"></a>Abilitare il banner di individuazione di estensione

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Una nuova funzionalità disponibile in Windows Admin Center anteprima 1903 è il banner di individuazione di estensione. Questa funzionalità consente a un'estensione dichiarare il produttore dell'hardware del server e supporta i modelli e quando un utente si connette a un server o cluster per il quale è disponibile un'estensione, verrà visualizzato un banner di notifica per installare facilmente l'estensione. Estensione per gli sviluppatori potranno ottenere maggiore visibilità per le relative estensioni e gli utenti saranno in grado di individuare facilmente altre funzionalità di gestione per i server.

![Banner di individuazione di estensione](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>Come funziona il banner di individuazione di estensione

All'avvio di Windows Admin Center, che verrà connettersi ai feed estensione registrate e recuperare i metadati per i pacchetti di estensione disponibili. Quindi, quando un utente si connette a un server o cluster in Windows Admin Center, viene letto il produttore dell'hardware del server e il modello da visualizzare nello strumento di panoramica. Se si trova un'estensione che dichiara il supporto produttore e/o modello del server corrente, verrà visualizzato un banner per informare l'utente. Facendo clic sul collegamento "Configura ora" l'utente passerà al gestore estensioni del quale è possibile installare l'estensione.

## <a name="how-to-implement-the-extension-discovery-banner"></a>Come implementare il banner di individuazione di estensione

I metadati "tags" nel file con estensione nuspec viene usato per dichiarare quali produttore dell'hardware e/o supportate dall'estensione di modelli. I tag sono delimitati da spazi ed è possibile aggiungere sia un produttore di tag del modello e/o per dichiarare il produttore supportati e/o i modelli. Il formato di tag viene ``"[value type]_[value condition]"`` in cui [valore] è di tipo "Produttore" o "Model" (maiuscole / minuscole) e [valore condizione] è un [espressione regolare di Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) che definisce il produttore o stringa di modello e [tipo di valore] e [ condizione del valore] sono separati da un carattere di sottolineatura. Questa stringa viene quindi codificata tramite URI codifica e aggiunto alla stringa di metadati con estensione nuspec "tags".

### <a name="example"></a>Esempio

Si supponga che ho sviluppato un'estensione che supporta i server da una società denominata Contoso Inc., con il modello nome R3xx e R4xx.

1. I tag per il produttore potrebbe essere ``"Manufacturer_/Contoso Inc./"``. Il tag per i modelli potrebbe essere ``"Model_/^R[34][0-9]{2}$/"``. A seconda di severità con cui si desidera definire la condizione di corrispondenza, saranno presenti diversi modi per definire l'espressione regolare. È anche possibile separare i tag di produttore o modello in più tag, ad esempio, il tag del modello è stata inoltre ``"Model_/R3../ Model_/R4../"``.
2. È possibile testare l'espressione regolare con la DevTools Console del web browser. In Edge o Chrome, premere F12 per aprire la finestra DevTools e nella scheda della Console, digitare il seguente e premere INVIO:

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   Se si digita ed eseguire il comando seguente, verrà restituito "true".

   ```javascript
   regex.test('R300')
   ```

   E se eseguire il comando seguente, verrà restituito 'false'.

   ```javascript
   regex.test('R500')
   ```

3. Dopo aver verificato l'espressione regolare, è possibile codificarli nella Console di DevTools anche, tramite il metodo Javascript seguente:

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   Il formato della stringa di tag da aggiungere al file con estensione nuspec finale sarà:

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> Siamo consapevoli che un produttore di hardware può avere un'ampia gamma di nomi di modello di cui alcune potrebbero essere supportate mentre altri non lo sono. Tenere presente che questa funzionalità è progettata per facilitare la **individuazione** dell'estensione, ma non deve essere un inventario di tutti i modelli aggiornato perfettamente. È possibile definire l'espressione regolare per essere un'espressione più semplice che corrisponde a un subset dei modelli. Un utente potrebbe non vedere il banner di individuazione se prima si connettono a un modello di server che non soddisfano la condizione, ma prima o poi si connetteranno a un altro server che e sarà individuare e installare l'estensione. È anche possibile definire una semplice espressione regolare che corrisponde solo il nome del produttore. In alcuni casi, l'estensione potrebbe non effettivamente supportare un modello specifico, ma è possibile usare la [funzionalità di visualizzazione degli strumenti dinamica](./dynamic-tool-display.md) per definire uno script di PowerShell personalizzato per verificare il supporto del modello e visualizzare solo l'estensione quando applicabile, o fornire funzionalità limitate nell'estensione per i modelli che non supportano tutte le funzionalità.