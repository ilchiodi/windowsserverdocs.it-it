---
title: Modifiche apportate a Nano Server in Windows Server Canale semestrale
description: Nel nuovo modello di manutenzione di Windows Server, Nano Server è solo un sistema operativo contenitore, che include alcune modifiche delle funzionalità.
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 05/21/2019
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: c9fede02b90e285803a8bcdbc983f264d65a4589
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976509"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Modifiche apportate a Nano Server in Windows Server Canale semestrale

>Si applica a: Windows Server, come canale semestrale

Se si esegue già Nano Server, il [canale semestrale Server finestra](..\get-started-19\servicing-channels-19.md) modello di manutenzione risulterà familiare, poiché è stata precedentemente provvedeva inoltre da Current Branch per il modello di Business (CBB). Canale semestrale di Windows Server è semplicemente un nuovo nome per lo stesso modello. In questo modello, i rilasci degli aggiornamenti di funzionalità di Nano Server sono previsti due o tre volte l'anno.

Tuttavia, a partire da Windows Server, versione 1803, Nano Server è disponibile solo come un **immagine del sistema operativo base contenitore**. Deve essere eseguito come contenitore in un host contenitore, come un'installazione Server Core di Windows Server. L'esecuzione di un contenitore basato su Nano Server in questa versione si differenzia dalle versioni precedenti per tre aspetti:

- Nano Server è stato ottimizzato per le applicazioni .NET Core.
- Nano Server è ancora più piccolo rispetto alla versione Windows Server 2016.
- PowerShell Core, .NET Core e WMI non sono più inclusi per impostazione predefinita, tuttavia è possibile includere i pacchetti contenitori [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) e .[NET Core](https://hub.docker.com/r/microsoft/dotnet/) durante la creazione del contenitore.
- In Nano Server non è più incluso uno stack di manutenzione. Microsoft pubblica un contenitore Nano aggiornato nell'hub Docker da ridistribuire.
- I problemi del nuovo contenitore Nano vengono risolti tramite Docker.
- È ora possibile eseguire contenitori Nano in IoT Core.

## <a name="related-topics"></a>Argomenti correlati

- [Documentazione di contenitori di Windows](http://aka.ms/windowscontainers)
- [Canale semestrale Server finestra Panoramica](..\get-started-19\servicing-channels-19.md)
