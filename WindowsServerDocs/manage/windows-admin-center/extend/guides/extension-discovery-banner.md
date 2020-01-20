---
title: Abilitazione del banner di individuazione delle estensioni
description: Abilitazione del banner di individuazione delle estensioni
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d761ba61ae5680373c334889799e82e5d092a0d4
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950103"
---
# <a name="enabling-the-extension-discovery-banner"></a>Abilitazione del banner di individuazione delle estensioni

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Una nuova funzionalità disponibile in Windows Admin Center Preview 1903 è il banner di individuazione delle estensioni. Questa funzionalità consente a un'estensione di dichiarare il produttore dell'hardware del server e i modelli che supporta e quando un utente si connette a un server o a un cluster per cui è disponibile un'estensione, viene visualizzato un banner di notifica per installare facilmente l'estensione. Gli sviluppatori di estensioni saranno in grado di ottenere una maggiore visibilità per le estensioni e gli utenti saranno in grado di individuare facilmente più funzionalità di gestione per i server.

![Banner di individuazione delle estensioni](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>Funzionamento del banner di individuazione delle estensioni

Quando viene avviato, l'interfaccia di amministrazione di Windows si connette ai feed di estensione registrati e recupera i metadati per i pacchetti di estensione disponibili. Quando un utente si connette a un server o a un cluster nell'interfaccia di amministrazione di Windows, viene letto il produttore e il modello dell'hardware del server per la visualizzazione nello strumento di panoramica. Se si rileva un'estensione che dichiara di supportare il produttore e/o il modello del server corrente, verrà visualizzato un banner per informare l'utente. Se si fa clic sul collegamento "Configura ora", l'utente verrà utilizzato da Gestione estensioni per l'installazione dell'estensione.

## <a name="how-to-implement-the-extension-discovery-banner"></a>Come implementare il banner di individuazione delle estensioni

I metadati "Tags" nel file con estensione NuSpec vengono usati per dichiarare il produttore e/o i modelli di hardware supportati dall'estensione. I tag sono delimitati da spazi ed è possibile aggiungere un tag del produttore o del modello oppure entrambi per dichiarare il produttore e/o i modelli supportati. Il formato del tag è ``"[value type]_[value condition]"`` dove [tipo valore] è "produttore" o "modello" (distinzione tra maiuscole e minuscole) e [condizione valore] è un' [espressione regolare JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Regular_Expressions) che definisce il produttore o la stringa del modello e [tipo valore] e [condizione valore] sono separate da un carattere di sottolineatura. Questa stringa viene quindi codificata utilizzando la codifica URI e aggiunta alla stringa di metadati "Tags". NuSpec.

### <a name="example"></a>Esempio

Supponiamo di aver sviluppato un'estensione che supporta I server di una società denominata Contoso Inc., con il nome del modello R3xx e R4xx.

1. Il tag per il produttore verrebbe ``"Manufacturer_/Contoso Inc./"``. Il tag per i modelli potrebbe essere ``"Model_/^R[34][0-9]{2}$/"``. A seconda del modo in cui si desidera definire la condizione di corrispondenza, sono disponibili diversi modi per definire l'espressione regolare. È anche possibile separare i tag del produttore o del modello in più tag. ad esempio, il tag del modello potrebbe essere ``"Model_/R3../ Model_/R4../"``.
2. È possibile testare l'espressione regolare con la console DevTools del Web browser. In Microsoft Edge o Chrome premere F12 per aprire la finestra DevTools e nella scheda console digitare quanto segue e premere INVIO:

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   Quindi, se si digita ed Esegui il comando seguente, verrà restituito ' true '.

   ```javascript
   regex.test('R300')
   ```

   E se si esegue quanto segue, verrà restituito ' false '.

   ```javascript
   regex.test('R500')
   ```

3. Una volta verificata l'espressione regolare, è possibile codificarla anche nella console di DevTools, usando il metodo JavaScript seguente:

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   Il formato finale della stringa di tag da aggiungere al file con estensione NuSpec sarà:

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> Un produttore di hardware può avere una gamma molto ampia di nomi di modelli di cui alcuni potrebbero essere supportati, mentre altri non lo sono. Tenere presente che questa funzionalità è concepita per aiutare a individuare **l'estensione** , ma non è necessario che sia un inventario perfettamente aggiornato di tutti i modelli. È possibile definire un'espressione regolare come espressione più semplice che corrisponde a un subset di modelli. Un utente potrebbe non visualizzare il banner di individuazione se si connette per la prima volta a un modello server che non corrisponde alla condizione, ma prima o poi si connetterà a un altro server che consente di individuare e installare l'estensione. È anche possibile definire una semplice espressione regolare che corrisponda solo al nome del produttore. In alcuni casi, l'estensione potrebbe non supportare effettivamente un modello specifico, ma è possibile usare la [funzionalità di visualizzazione dello strumento dinamico](./dynamic-tool-display.md) per definire uno script di PowerShell personalizzato per verificare il supporto dei modelli e visualizzare l'estensione solo quando applicabile oppure fornire funzionalità limitate nell'estensione per i modelli che non supportano tutte le funzionalità.