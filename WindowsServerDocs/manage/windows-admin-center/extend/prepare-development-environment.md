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
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080918"
---
# Preparare l'ambiente di sviluppo

>Si applica a: Windows Admin Center, Anteprima di Windows Admin Center

Iniziamo a sviluppare estensioni con Windows Admin Center SDK.  In questo documento viene descritto il processo per ottenere l'ambiente e creare e testare un'estensione per Windows Admin Center.

> [!NOTE]
> Non conosci Windows Admin Center SDK?  Ulteriori informazioni sulle [Estensioni per Windows Admin Center](extensibility-overview.md)

Per preparare l'ambiente di sviluppo, effettua i passaggi seguenti:

## Installare i prerequisiti

Per iniziare a sviluppare con l'SDK, scarica e installa i prerequisiti seguenti:

* [Windows Admin Center](https://aka.ms/WACDownloadPage) (Versione GA o preview)
* Visual Studio o [Visual Studio Code](http://code.visualstudio.com)
* [Gestione pacchetti del nodo](https://npmjs.com/get-npm) (8.12.0 o versioni successive)
* [NuGet](https://www.nuget.org/downloads) (per la pubblicazione delle estensioni)

> [!NOTE]
> È necessario installare ed eseguire Windows Admin Center in modalità sviluppatore per effettuare i passaggi seguenti. La modalità sviluppatore consente a Windows Admin Center di caricare pacchetti di estensioni non firmati.
>
>  Per abilitare la modalità sviluppatore, installa Windows Admin Center dalla riga di comando con il parametro DEV_MODE=1. Nell'esempio seguente, sostituisci ```<version>``` con la versione che stai installando, ad esempio ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## Installare le dipendenze globale

Successivamente, installa o aggiorna le dipendenze necessarie per i progetti, con Gestione pacchetti del nodo. Queste dipendenze verranno installate a livello globale e saranno disponibili per tutti i progetti.

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>È possibile installare una versione successiva di @angular/cli, tuttavia tenere presente che se si installa una versione superiore a 1.6.5, riceverai un avviso durante il passaggio di compilazione gulp che la versione locale cli non corrisponde la versione installata.

## Passaggi successivi

Ora che l'ambiente è pronto, sei pronto per iniziare a creare il contenuto.

- Creare un'estensione dello [strumento](develop-tool.md)
- Creare un'estensione della [soluzione](develop-solution.md)
- Creare un [plug-in gateway](develop-gateway-plugin.md)
- Ulteriori informazioni nelle [Guide](guides.md)

## Toolkit di progettazione SDK

Guarda il nostro Windows Admin Center [SDK toolkit di progettazione](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)! Il toolkit è progettato per aiutarti a simulazione rapidamente le estensioni PowerPoint usando stili Windows Admin Center, controlli e modelli di pagina. Vedi l'estensione l'aspetto in Windows Admin Center prima di iniziare a scrivere codice!

