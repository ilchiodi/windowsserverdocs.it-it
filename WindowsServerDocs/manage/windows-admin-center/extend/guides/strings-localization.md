---
title: Le stringhe e la localizzazione in Windows Admin Center
description: Informazioni sulle operazioni preliminari per la localizzazione delle stringhe in Windows Admin Center SDK (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fb328f86c98141a5a2a1c4fd05ec1d4c96a7bc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845402"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Le stringhe e la localizzazione in Windows Admin Center #

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Andiamo più approfondita in Windows Admin Center estensioni SDK e discutere le stringhe e della localizzazione.

Per abilitare la localizzazione di tutte le stringhe che vengono sottoposti a rendering nel livello di presentazione, Ottieni i vantaggi del file strings.resjson sotto /src/resources/strings - si è già configurato. Quando è necessario aggiungere una nuova stringa per l'estensione, è necessario aggiungerlo a questo file resjson come una nuova voce. La struttura esistente il formato seguente:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

È possibile usare qualsiasi formato desiderato per le stringhe, ma tenere presente che il processo di generazione, il processo che accetta il resjson e restituisce la classe di TypeScript utilizzabile, converte un carattere di sottolineatura (_) in punti (.).

Ad esempio, questa voce:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Genera la struttura della funzione di accesso seguente:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```
