---
title: Le stringhe e localizzazione in Windows Admin Center
description: Scopri come ottenere le stringhe pronto per la localizzazione in Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f0671160bd790d80e35f6d6c1586a07969939070
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296733"
---
# Le stringhe e localizzazione in Windows Admin Center #

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Soffermeremo più accurato in Windows Admin Center estensioni SDK e parlare stringhe e localizzazione.

Per consentire la localizzazione di tutte le stringhe che vengono visualizzati nel livello di presentazione, sfruttare i vantaggi del file strings.resjson in /src/resources/strings - già configurato. Quando è necessario aggiungere una nuova stringa alla tua estensione, aggiungerlo a questo file resjson come una nuova voce. La struttura esistente segue questo formato:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

È possibile utilizzare qualsiasi formato che come per le stringhe, ma Tieni presente che il processo di generazione (il processo che accetta il resjson e genera la classe TypeScript utilizzabile) converte carattere di sottolineatura in punti (.).

Ad esempio, questa voce:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Genera la struttura della funzione di accesso seguenti:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## Aggiungere altre lingue per la localizzazione ## 

Per la localizzazione in altri linguaggi, un file strings.resjson deve essere creato per ogni lingua. Questi file devono essere posizioni nel ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```. Le lingue disponibili con cartelle corrispondenti sono:

| Lingua      | Cartella      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Tedesco | de-DE |
| Inglese | en-US |
| Come | es-ES |
| Français | fr-FR | 
| Magyar | hu-HU | 
| Italiano | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| Paesi Bassi | nl-NL |
| Polski | pl-PL |
| Português (Brasile) | pt-BR |
| Português (Portogallo) | pt-PT |
| РУССКИЙ | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> Se le esigenze di struttura di file sono diverse all'interno del localizzate/output, devi regolare il localeOffset per l'attività di gulp 'generare resjson-json-localizzata' che è il gulpfile.js. Questo offset è il livello di profondità nella cartella localizzate, deve iniziare la ricerca dei file strings.resjson.

Ogni file strings.resjson verrà formattata nello stesso modo come indicato in precedenza nella parte superiore di questa Guida. 

Ad esempio, per includere una localizzazione per come includere questa voce nel ```\loc\output\HelloWorld\es-ES\strings.resjson```: 
```json
"HelloWorld_cim_title": "CIM Componente",
```
In qualsiasi momento che hai aggiunto le stringhe localizzate, gulp generare deve essere eseguito nuovamente per visualizzarli. Esegui:
``` cmd
gulp generate 
```

Per verificare che le stringhe siano state generate passa a ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```. In questo file verrà visualizzata la voce appena aggiunta.
Ora se passi l'opzione di lingua in Windows Admin Center, sarai in grado di visualizzare le stringhe localizzate nell'estensione. 
