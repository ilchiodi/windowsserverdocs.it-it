---
title: Introduzione a Windows Server, versione 1803
description: Istruzioni per l'acquisizione, l'installazione e l'attivazione
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 05/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: c0a4917d0fdb3e911204601d6137d8c8a296e57a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812282"
---
# <a name="introducing-windows-server-version-1803"></a>Introduzione a Windows Server, versione 1803

>Si applica a: Windows Server (Canale semestrale)

**Windows Server, versione 1803 è la versione corrente nel nuovo canale semestrale**


## <a name="what-the-semi-annual-channel-is--and-isnt"></a>Informazioni sul Canale semestrale
Windows Server versione 1803 *non* è un "aggiornamento" o "service pack" per Windows Server 2016. È la versione semestrale corrente del server nel programma di rilascio progettata per i clienti che stanno passando a una "pianificazione cloud" tramite cicli di sviluppo rapido. Questo programma è ideale per applicazioni moderne e scenari di innovazione quali contenitori e micro servizi. Ogni versione rilasciata nell'ambito di questo programma è supportata per 18 mesi a partire dal rilascio iniziale. Per ulteriori informazioni sul Canale semestrale e **suggerimenti per decidere a quale prendere parte** (o quale mantenere) vedi [Panoramica del Canale semestrale](semi-annual-channel-overview.md).


**Windows Server 2016 è il prodotto di Long-Term Servicing Channel (LTSC) corrente.** Il Canale di manutenzione a lungo termine è ideale se hai bisogno di stabilità e prevedibilità a lungo termine nel sistema operativo del server, al fine di supportare applicazioni e carichi di lavoro tradizionali. Se desideri mantenere il canale LTSC, devi installare (o continuare a usare) Windows Server 2016, che può essere installato in modalità Server Core o Server con la modalità Esperienza desktop. Vedi [Introduzione a Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics) per informazioni dettagliate.


## <a name="whats-different-about-windows-server-version-1803"></a>Che differenze presenta Windows Server, versione 1803?

Windows Server, versione 1803 viene eseguito in modalità Server Core. La modalità Windows Server Core offre diversi vantaggi, ad esempio i requisiti hardware inferiori, superficie di attacco più piccola e una riduzione nella necessità di aggiornamenti. Poiché non dispone di alcuna interfaccia utente grafica, la modalità Windows Server Core viene gestita meglio in remoto. Se non hai esperienza con l'utilizzo di Server Core, le informazioni relative alla [gestione di Server Core](../administration/server-core/server-core-manage.md) ti consentiranno di acquisire familiarità con questo ambiente. Le informazioni sulla [gestione di Windows Server 2016](../administration/manage-windows-server.md) mostrano varie opzioni per la gestione remota dei server.

[Novità di Windows Server, versione 1803](whats-new-in-windows-server-1803.md) offre un'introduzione alle nuove funzionalità e alle funzioni aggiunte in Windows Server, versione 1803.

### <a name="why-does-windows-server-version-1803-offer-only-the-server-core-installation-option"></a>Perché Windows Server versione 1803 offre solo l'opzione di installazione Server Core?
Uno degli approcci più importanti adottati nella pianificazione di ogni versione di Windows Server implica l'ascolto del feedback dei clienti: in che modo viene utilizzato Windows Server? Quali nuove funzionalità avranno l'impatto maggiore sulle distribuzioni di Windows Server e, di conseguenza, sulle attività quotidiane? Il feedback dei clienti indica che la distribuzione di innovazioni nel modo più veloce ed efficiente possibile è una priorità chiave. Allo stesso tempo, i clienti che innovano più rapidamente indicano di utilizzare principalmente script dalla riga di comando con PowerShell per gestire i data center e di conseguenza non hanno grande necessità di avere a disposizione un'interfaccia utente grafica desktop nell'installazione di Windows Server con esperienza desktop. Concentrandosi sull'opzione di installazione Server Core, è possibile dedicare più risorse alle innovazioni, pur mantenendo la compatibilità con le tradizionali applicazioni e funzionalità della piattaforma Windows Server. I commenti e suggerimenti su questo o altri problemi relativi a Windows Server e alle versioni future possono essere inviati tramite l'[Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).


### <a name="what-about-nano-server"></a>Informazioni su Nano Server
Nano server è disponibile come sistema operativo contenitore. Vedere [Modifiche a Nano Server nel Canale semestrale di Windows](nano-in-semi-annual-channel.md) per informazioni dettagliate.

## <a name="additional-information-about-this-release"></a>Informazioni aggiuntive su questa versione
Per ottenere una panoramica completa degli aspetti chiave su Windows Server, versione 1803, è consigliabile consultare anche questi argomenti prima di eseguire l'installazione:

- Che tipo di hardware è necessario per eseguirlo? Vedi [Requisiti di sistema](system-requirements.md). I requisiti di sistema per questa versione sono uguali a quelli per Windows Server 2016.
- Quali nuove caratteristiche e funzionalità sono state aggiunte? Vedere [Novità di Windows Server, versione 1803](whats-new-in-windows-server-1803.md)
- Cosa è stato rimosso? Vedi [funzionalità rimosse o pianificate per la sostituzione a partire da Windows Server, versione 1803](windows-server-1803-removed-features.md)
- Quali problemi specifici di questa versione devono essere risolti? Vedi [Note sulla versione: problemi importanti di Windows Server, versione 1803](server-1803-release-notes.md)


## <a name="where-to-obtain-windows-server-version-1803"></a>Come ottenere Windows Server, versione 1803

Questa versione deve essere installata con un'installazione pulita.

- Volume Licensing Service Center (VLSC): I clienti con contratto multilicenza con [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) può ottenere questa versione visitando il [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) e fare clic su **Accedi**. Quindi possono fare clic su **Download e chiavi** e cercare questa versione. 

- Windows Server, versione 1803 è disponibile anche in [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Sottoscrizioni di Visual Studio: Sottoscrittori di Visual Studio è possibile ottenere Windows Server, versione 1803, scaricarlo dal [pagina di download di Visual Studio sottoscrittore](https://my.visualstudio.com/downloads?pid=2347). Se non si è già un sottoscrittore, passare a [Sottoscrizioni di Visual Studio](https://www.visualstudio.com/subscriptions/) per registrarsi e quindi visitare la [pagina di download del Sottoscrittore di Visual Studio](https://my.visualstudio.com/downloads?pid=2347) come indicato in precedenza. Le versioni ottenute tramite le sottoscrizioni di Visual Studio sono esclusivamente a scopo di sviluppo e test.




## <a name="activating-windows-server-version-1803"></a>Attivazione di Windows Server, versione 1803

- Se hai ottenuto questa versione dal Volume Licensing Service Center, puoi attivarla tramite il CSVLK di Windows Server 2016 con l'ambiente di sistema di gestione delle chiavi (KMS).
- Se utilizzi Microsoft Azure, questa versione viene automaticamente attivata.
- Se ottieni questa versione dalle sottoscrizioni di Visual Studio, puoi attivarla tramite il CSVLK di Windows Server 2016 con l'ambiente di sistema di gestione delle chiavi (KMS). 
