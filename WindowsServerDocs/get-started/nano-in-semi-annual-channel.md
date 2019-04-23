---
title: Modifiche apportate a Nano Server in Windows Server Canale semestrale
description: Nel nuovo modello di manutenzione di Windows Server, Nano Server è solo un sistema operativo contenitore, che include alcune modifiche delle funzionalità.
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: medium
ms.date: 05/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: 7e68d292c32ce58c786a3242203330fcae985913
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847772"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Modifiche apportate a Nano Server in Windows Server Canale semestrale

>Si applica a: Windows Server, come canale semestrale


Come descritto in [Panoramica del canale semestrale di Windows Server](semi-annual-channel-overview.md), Windows Server versione 1803 è la versione più recente nel canale semestrale.

Se hai già in esecuzione Nano Server, questo modello di servizio ti sarà familiare, poiché è stato precedentemente fornito dal modello Current Branch for Business (CBB). Il nuovo canale semestrale di Windows Server non è altro che lo stesso modello, con un nuovo nome. In questo modello, i rilasci degli aggiornamenti di funzionalità di Nano Server sono previsti due o tre volte l'anno.

In questa versione di Windows Server, versione 1803, tuttavia, Nano Server è disponibile solo come un' **immagine del sistema operativo di base contenitore**. Deve essere eseguito come contenitore in un host contenitore, come un'installazione Server Core di Windows Server. L'esecuzione di un contenitore basato su Nano Server in questa versione si differenzia dalle versioni precedenti per tre aspetti:

- Nano Server è stato ottimizzato per le applicazioni .NET Core.
- Nano Server è ancora più piccolo rispetto alla versione Windows Server 2016.
- PowerShell Core, .NET Core e WMI non sono più inclusi per impostazione predefinita, tuttavia è possibile includere i pacchetti contenitori [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) e .[NET Core](https://hub.docker.com/r/microsoft/dotnet/) durante la creazione del contenitore.
- In Nano Server non è più incluso uno stack di manutenzione. Microsoft pubblica un contenitore Nano aggiornato nell'hub Docker da ridistribuire.
- I problemi del nuovo contenitore Nano vengono risolti tramite Docker.
- È ora possibile eseguire contenitori Nano in IoT Core.

## <a name="related-topics"></a>Argomenti correlati
Quando viene avviato il Programma Insider, è possibile trovare ulteriori informazioni nella [documentazione dei contenitori di Windows](http://aka.ms/windowscontainers).
