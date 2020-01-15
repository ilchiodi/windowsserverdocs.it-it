---
title: Preparare l'ambiente di sviluppo
description: Preparazione dell'ambiente di sviluppo di Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server
ms.openlocfilehash: 67bd2a476cedd6d522daeaae54081b02fd893fbd
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949971"
---
# <a name="prepare-your-development-environment"></a>Preparare l'ambiente di sviluppo

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Iniziamo a sviluppare le estensioni con Windows Admin Center SDK.  In questo documento verrà illustrata la procedura per far funzionare l'ambiente per compilare e testare un'estensione per l'interfaccia di amministrazione di Windows.

> [!NOTE]
> Non conosci Windows Admin Center SDK?  Ulteriori informazioni sulle [Estensioni per Windows Admin Center](extensibility-overview.md)

Per preparare l'ambiente di sviluppo, effettua i passaggi seguenti:

## <a name="install-prerequisites"></a>Installare i prerequisiti

Per iniziare a sviluppare con l'SDK, scarica e installa i prerequisiti seguenti:

* Interfaccia di [amministrazione di Windows](https://aka.ms/WACDownloadPage) (versione GA o versione di anteprima)
* Visual Studio o [Visual Studio Code](https://code.visualstudio.com)
* [Gestione pacchetti di nodi](https://npmjs.com/get-npm) (8.12.0 o versione successiva)
* [NuGet](https://www.nuget.org/downloads) (per la pubblicazione delle estensioni)

> [!NOTE]
> È necessario installare ed eseguire Windows Admin Center in modalità sviluppatore per effettuare i passaggi seguenti. La modalità sviluppatore consente a Windows Admin Center di caricare pacchetti di estensioni non firmati.
>
>  Per abilitare la modalità sviluppatore, installa Windows Admin Center dalla riga di comando con il parametro DEV_MODE=1. Nell'esempio seguente, sostituisci ```<version>``` con la versione che stai installando, ad esempio ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>Installare le dipendenze globali

Successivamente, installare o aggiornare le dipendenze necessarie per i progetti, con gestione pacchetti del nodo. Queste dipendenze verranno installate a livello globale e saranno disponibili per tutti i progetti.

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>È possibile installare una versione più recente di @angular/cli, tuttavia tenere presente che se si installa una versione maggiore di 1.6.5, si riceverà un avviso durante il passaggio di compilazione Gulp che la versione dell'interfaccia della riga di comando locale non corrisponde alla versione installata.

## <a name="next-steps"></a>Passaggi successivi

Ora che l'ambiente è pronto, è possibile iniziare a creare il contenuto.

- Creare un'estensione dello [strumento](develop-tool.md)
- Creare un'estensione della [soluzione](develop-solution.md)
- Creare un [plug-in gateway](develop-gateway-plugin.md)
- Ulteriori informazioni nelle [Guide](guides.md)

## <a name="sdk-design-toolkit"></a>Toolkit di progettazione SDK

Visitare il Windows Admin Center [SDK Design Toolkit](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip). Questo Toolkit è stato progettato per consentire di simulare rapidamente le estensioni in PowerPoint usando gli stili, i controlli e i modelli di pagina dell'interfaccia di amministrazione di Windows. Prima di iniziare a scrivere il codice, vedere l'aspetto dell'estensione nell'interfaccia di amministrazione di Windows.

