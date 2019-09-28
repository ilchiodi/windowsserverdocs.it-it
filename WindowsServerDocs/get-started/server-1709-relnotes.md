---
title: 'Note sulla versione: problemi importanti di Windows Server versione 1709'
description: Riepilogano i problemi critici che richiedono soluzioni alternative per evitare l'arresto anomalo del sistema, i blocchi, gli errori di installazione o la perdita di dati.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 04/23/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 7d5f899964414c10350cc22a594a959c940a1514
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391516"
---
# <a name="release-notes-important-issues-in-windows-server-version-1709"></a>Note sulla versione: problemi importanti di Windows Server versione 1709

>Si applica a: Canale semestrale Windows Server

Queste note sulla versione presentano una sintesi dei problemi più critici del sistema operativo Windows Server&reg;, incluse le soluzioni per evitare o risolvere i problemi, se noti. Per informazioni sulle modifiche da progettazione, le nuove funzionalità e le correzioni apportate in questa versione, vedi [Novità di Windows Server, versione 1709](whats-new-in-windows-server-1709.md) e gli annunci dei team addetti alle funzionalità specifiche. Se non diversamente specificato, ogni problema segnalato è applicabile a tutte le edizioni e opzioni di installazione di Windows Server 2016.  

Questo documento viene continuamente aggiornato. Non appena vengono rilevati problemi critici, le informazioni correlate vengono aggiunte immediatamente insieme alle soluzioni e alle correzioni disponibili.  
  
## <a name="storage-spaces-direct"></a>Spazi di archiviazione diretta
[comment]: # (ID: sconosciuto; mittente: stevenek; stato: approvato)  
La funzionalità Spazi di archiviazione diretta non è inclusa in Windows Server versione 1709. Se chiami *Enable-ClusterStorageSpacesDirect* o il relativo alias *Enable-ClusterS2D*, in un server che esegue Windows Server versione 1709, riceverai un errore con il messaggio "Operazione richiesta non supportata".

Non è inoltre supportata per introdurre i server che eseguono Windows Server versione 1709 in una distribuzione di Spazi di archiviazione diretta di Windows Server 2016.

Il modello di rilascio di Windows Server offre una nuova possibilità di aggiornamento che ne permette l'allineamento a versioni e modelli di manutenzione simili per [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) e [Office 365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). I rilasci del Canale semestrale offrono nuove funzionalità ai clienti che vogliono passare a una frequenza di aggiornamento rapida e che potranno usufruire di nuovi rilasci due volte all'anno, in primavera e in autunno.

Il Canale semestrale di Windows Server è incentrato su scenari relativi a contenitori e applicazioni che traggono vantaggio dall'introduzione più rapida delle innovazioni. Per altre informazioni, vedi questo [blog](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update). I clienti che cercano ruoli di infrastruttura, ad esempio Spazi di archiviazione diretta, devono usare versioni Long-Term Servicing Channel, ad esempio Windows Server 2016 (ora disponibile) e [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (disponibile nel corso dell'anno). Siamo impegnati a creare una piattaforma di altissimo livello per l'infrastruttura iperconvergente e continuiamo a sviluppare nuove funzionalità e a migliorare quelle esistenti in base ai commenti ricevuti. 

La funzionalità Spazi di archiviazione diretta è stata introdotta in Windows Server 2016 ed è alla base della piattaforma iperconvergente. Siamo entusiasti dell'accoglienza positiva della piattaforma Microsoft iperconvergente e desideriamo offrire il massimo ai nostri clienti.

Abbiamo prestato attenzione ai commenti ricevuti e stiamo lavorando per offrire [la prossima serie di innovazioni](https://blogs.technet.microsoft.com/windowsserver/2017/09/07/sneak-peek-2-windows-server-version-1709-hyper-converged-infrastructure/) per la piattaforma iperconvergente. Queste funzionalità sono attualmente disponibili nelle build per i [partecipanti al programma Windows Insider](https://insider.windows.com/for-business/). Siamo lieti di offrirti la possibilità di provarle e condividere i tuoi commenti. Per i clienti che cercano una soluzione iperconvergente convalidata, consigliamo il programma [software-defined per Windows Server](http://microsoft.com/wssd).
