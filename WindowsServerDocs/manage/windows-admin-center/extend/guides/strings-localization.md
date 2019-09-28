---
title: Stringhe e localizzazione nell'interfaccia di amministrazione di Windows
description: Informazioni su come ottenere le stringhe pronte per la localizzazione in Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 61289ae175ca8b906386cff9e36f5023ea28d051
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385244"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Stringhe e localizzazione nell'interfaccia di amministrazione di Windows #

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Esaminiamo in dettaglio l'SDK di Windows Admin Center Extensions e parlo di stringhe e localizzazione.

Per abilitare la localizzazione di tutte le stringhe di cui viene eseguito il rendering sul livello di presentazione, sfruttare il file Strings. resjson in/src/Resources/Strings. è già configurato. Quando è necessario aggiungere una nuova stringa all'estensione, aggiungerla al file resjson come nuova voce. La struttura esistente segue il formato seguente:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

È possibile usare qualsiasi formato per le stringhe, ma tenere presente che il processo di generazione (il processo che accetta resjson e restituisce la classe TypeScript utilizzabile) converte il carattere di sottolineatura (_) in punti (.).

Ad esempio, questa voce:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Genera la seguente struttura della funzione di accesso:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## <a name="add-other-languages-for-localization"></a>Aggiungere altre lingue per la localizzazione ## 

Per la localizzazione in altre lingue, è necessario creare un file Strings. resjson per ogni lingua. Questi file devono essere posizionati in ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```. Le lingue disponibili con le cartelle corrispondenti sono:

| Linguaggio      | Cartella      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Deutsch | de-DE |
| Inglese | en-US |
| Español | es-ES |
| Français | fr-FR | 
| Magyar | hu-HU | 
| Italiano | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| Nederlands | nl-NL |
| Polski | pl-PL |
| Português (Brasil) | pt-BR |
| Português (Portugal) | pt-PT |
| Русский | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> Se le esigenze della struttura di file sono diverse all'interno di Loc/output, sarà necessario modificare il localeOffset per l'attività Gulp ' generate-resjson-JSON-localizzate ' che si trova in gulpfile. js. Questo offset è il modo in cui la cartella loc dovrebbe iniziare a cercare i file Strings. resjson.

Ogni file Strings. resjson verrà formattato in modo analogo a quanto indicato in precedenza nella parte superiore di questa guida. 

Ad esempio, per includere una localizzazione per Español, includere questa voce ```\loc\output\HelloWorld\es-ES\strings.resjson```in: 
```json
"HelloWorld_cim_title": "CIM Componente",
```
Ogni volta che sono state aggiunte stringhe localizzate, Gulp generate deve essere eseguito di nuovo per poter essere visualizzato. Eseguire:
``` cmd
gulp generate 
```

Per verificare che le stringhe siano state generate, ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```passare a. La voce appena aggiunta verrà visualizzata in questo file.
A questo punto, se si cambia l'opzione Language nell'interfaccia di amministrazione di Windows, sarà possibile visualizzare le stringhe localizzate nell'estensione. 
