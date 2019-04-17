---
title: Introduzione a Windows Server, versione 1709
description: Istruzioni per l'acquisizione, l'installazione e l'attivazione
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 12/5/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: a2da7b8437a9dc7f44fc83837e3ebad93b7fe3ab
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "1410401"
---
# <a name="introducing-windows-server-version-1709"></a>Introduzione a Windows Server, versione 1709

>Si applica a: Windows Server (Canale semestrale)

**Windows Server, versione 1709 è la prima versione del nuovo Canale semestrale.** 

## <a name="what-the-semi-annual-channel-is--and-isnt"></a>Informazioni sul Canale semestrale
Come prima versione rilasciata in questo nuovo canale, Windows Server, versione 1709 *non* è un "aggiornamento" né "Service Pack" di Windows Server 2016. È la prima delle versioni semestrali del server in un **nuovo programma di rilascio** progettato per i clienti che si stanno muovendo verso una "cadenza cloud", ad esempio quelli con cicli di sviluppo rapidi o i provider di servizi di hosting che desiderano tenere il passo con gli investimenti in Hyper-V più recenti. Ogni versione rilasciata nell'ambito di questo programma è supportata per 18 mesi a partire dal rilascio iniziale. Per ulteriori informazioni sul Canale semestrale e **suggerimenti per decidere a quale prendere parte** (o quale mantenere) vedere [Panoramica del Canale semestrale](semi-annual-channel-overview.md).


**La versione corrente del Long-Term Servicing Channel (LTSC) è Windows Server 2016**. LTSC è ideale se hai bisogno di stabilità e prevedibilità a lungo termine nel sistema operativo server, al fine di supportare applicazioni e carichi di lavoro tradizionali. Se desideri mantenere il canale LTSC, devi installare (o continuare a usare) Windows Server 2016, che può essere installato in modalità Server Core o Server con la modalità Esperienza desktop. Vedere [Introduzione a Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics) per informazioni dettagliate.


## <a name="whats-different-about-1709"></a>Differenze nella versione 1709.

Windows Server, versione 1709 viene eseguito in modalità Server Core. Non esiste pertanto un'interfaccia utente grafica e la gestione viene eseguita in remoto. Offre tuttavia diversi vantaggi, ad esempio requisiti hardware ridotti, superficie di attacco molto più piccola e una ridotta necessità per gli aggiornamenti. Se non hai esperienza con l'utilizzo di Server Core, le informazioni relative alla [gestione di Server Core](../administration/server-core/server-core-manage.md) ti consentiranno di acquisire familiarità con questo ambiente. Le informazioni sulla [gestione di Windows Server 2016](../administration/manage-windows-server.md) mostrano varie opzioni per la gestione remota dei server.

[Novità di Windows Server, versione 1709](whats-new-in-windows-server-1709.md) offre un'introduzione alle nuove funzionalità e alle funzioni aggiunte in Windows Server, versione 1709.

### <a name="why-does-windows-server-version-1709-offer-only-the-server-core-installation-option"></a>Perché Windows Server versione 1709 offre solo l'opzione di installazione Server Core?
Uno degli approcci più importanti adottati nella pianificazione di ogni versione di Windows Server implica l'ascolto del feedback dei clienti: in che modo viene utilizzato Windows Server? Quali nuove funzionalità avranno l'impatto maggiore sulle distribuzioni di Windows Server e, di conseguenza, sulle attività quotidiane? Il feedback dei clienti indica che la distribuzione di innovazioni nel modo più veloce ed efficiente possibile è una priorità chiave. Allo stesso tempo, i clienti che innovano più rapidamente indicano di utilizzare principalmente script dalla riga di comando con PowerShell per gestire i data center e di conseguenza non hanno grande necessità di avere a disposizione un'interfaccia utente grafica desktop nell'installazione di Windows Server con esperienza desktop. Concentrandosi sull'opzione di installazione Server Core, è possibile dedicare più risorse alle innovazioni, pur mantenendo la compatibilità con le tradizionali applicazioni e funzionalità della piattaforma Windows Server. I commenti e suggerimenti su questo o altri problemi relativi a Windows Server e alle versioni future possono essere inviati tramite l'[Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).


### <a name="what-about-nano-server"></a>Informazioni su Nano Server
Nano server è disponibile come sistema operativo contenitore. Vedere [Modifiche a Nano Server nel Canale semestrale di Windows](nano-in-semi-annual-channel.md) per informazioni dettagliate.

## <a name="additional-information-about-this-release"></a>Informazioni aggiuntive su questa versione
Per ottenere una panoramica completa degli aspetti chiave su Windows Server, versione 1709, è consigliabile consultare anche questi argomenti prima di eseguire l'installazione:

- Che tipo di hardware è necessario per eseguirlo? Vedi [Requisiti di sistema](system-requirements.md). I requisiti di sistema per questa versione sono uguali a quelli per Windows Server 2016.
- Quali nuove caratteristiche e funzionalità sono state aggiunte? Vedere [Novità di Windows Server, versione 1709](whats-new-in-windows-server-1709.md)
- Cosa è stato rimosso? Vedi [Funzionalità rimosse o pianificate per la sostituzione a partire da Windows Server, versione 1709](Removed-Features-1709.md)
- Quali problemi specifici di questa versione devono essere risolti? Vedi [Note sulla versione: problemi importanti di Windows Server, versione 1709](server-1709-relnotes.md)


## <a name="where-to-obtain-windows-server-version-1709"></a>Come ottenere Windows Server, versione 1709

Questa versione deve essere installata con un'installazione pulita.

- VLSC: i clienti che dispongono di contratti multilicenza con [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) possono ottenere questa versione dal [Centro servizi per contratti multilicenza](https://www.microsoft.com/Licensing/servicecenter/default.aspx) e facendo clic su **Accedi**. Quindi possono fare clic su **Download e chiavi** e cercare questa versione. 

- Windows Server, versione 1709 è disponibile anche in [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- I partecipanti al programma **Sottoscrizioni di Visual Studio** possono ottenere Windows Server, versione 1709 accedendo alla [pagina di download del Sottoscrittore di Visual Studio](https://my.visualstudio.com/downloads?pid=2347) e completare il download ivi disponibile. Se non si è già un sottoscrittore, passare a [Sottoscrizioni di Visual Studio](https://www.visualstudio.com/subscriptions/) per registrarsi e quindi visitare la [pagina di download del Sottoscrittore di Visual Studio](https://my.visualstudio.com/downloads?pid=2347) come indicato in precedenza. Le versioni ottenute tramite Sottoscrizioni di Visual Studio sono esclusivamente a scopo di sviluppo e test.




## <a name="activating-windows-server-version-1709"></a>Attivazione di Windows Server, versione 1709

- Se questa versione è stata ottenuta tramite il Centro servizi per contratti multilicenza, è possibile attivarla usando il codice di attivazione di Windows Server 2016.
- Se utilizzi Microsoft Azure, questa versione viene automaticamente attivata.
- Se questa versione si ottiene da Sottoscrizioni di Visual Studio, dovrebbe essere automaticamente attivata.