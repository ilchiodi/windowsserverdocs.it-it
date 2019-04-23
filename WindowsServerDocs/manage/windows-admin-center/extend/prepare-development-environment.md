---
title: Preparare l'ambiente di sviluppo
description: Preparazione dell'ambiente di sviluppo di Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b1a0672ee374f3e2d1339c43576db0e5cabdc36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834762"
---
# <a name="prepare-your-development-environment"></a>Preparare l'ambiente di sviluppo

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Iniziamo a sviluppare estensioni con Windows Admin Center SDK.  In questo documento si affronterà il processo per ottenere l'ambiente di backup e per compilare e testare un'estensione per Windows Admin Center sia in esecuzione.

> [!NOTE]
> Non conosci Windows Admin Center SDK?  Ulteriori informazioni sulle [Estensioni per Windows Admin Center](extensibility-overview.md)

Per preparare l'ambiente di sviluppo, effettua i passaggi seguenti:

## <a name="install-prerequisites"></a>Installare i prerequisiti

Per iniziare a sviluppare con l'SDK, scarica e installa i prerequisiti seguenti:

* [Windows Admin Center](https://aka.ms/WACDownloadPage) (versione di anteprima o GA)
* Visual Studio o [Visual Studio Code](http://code.visualstudio.com)
* [Node Package Manager](https://npmjs.com/get-npm) (8.12.0 o versioni successive)
* [NuGet](https://www.nuget.org/downloads) (per la pubblicazione delle estensioni)

> [!NOTE]
> È necessario installare ed eseguire Windows Admin Center in modalità sviluppatore per effettuare i passaggi seguenti. La modalità sviluppatore consente a Windows Admin Center di caricare pacchetti di estensioni non firmati.
>
>  Per abilitare la modalità sviluppatore, installa Windows Admin Center dalla riga di comando con il parametro DEV_MODE=1. Nell'esempio seguente, sostituisci ```<version>``` con la versione che stai installando, ad esempio ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>Installare le dipendenze globale

Successivamente, installare o aggiornare le dipendenze necessarie per i progetti, con Node Package Manager. Queste dipendenze verranno installate a livello globale e saranno disponibili per tutti i progetti.

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>È possibile installare una versione successiva di @angular/cli, tuttavia occorre tenere presente che se si installa una versione maggiore 1.6.5, si riceverà un avviso durante il passaggio di compilazione gulp che la versione locale dell'interfaccia della riga non corrisponde alla versione installata.

## <a name="next-steps"></a>Passaggi successivi

Ora che l'ambiente è pronto, si è pronti per iniziare a creare il contenuto.

- Creare un'estensione dello [strumento](develop-tool.md)
- Creare un'estensione della [soluzione](develop-solution.md)
- Creare un [plug-in gateway](develop-gateway-plugin.md)
- Ulteriori informazioni nelle [Guide](guides.md)

## <a name="sdk-design-toolkit"></a>Toolkit di progettazione SDK

Consultare il Windows Admin Center [toolkit progettazione SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)! Questo toolkit è progettato per aiutarti a simulare rapidamente le estensioni in PowerPoint con stili di Windows Admin Center, controlli e modelli di pagina. Vedere l'estensione può come appaiono nel Windows Admin Center prima di iniziare a scrivere codice!

