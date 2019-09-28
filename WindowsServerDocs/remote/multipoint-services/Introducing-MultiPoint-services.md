---
title: Introduzione a Servizi MultiPoint
description: Viene fornita una panoramica di MultiPoint Services, un modo per consentire a più utenti di condividere un sistema
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: a0497f9dfd39648a94d9fb832f4404491955c06a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395350"
---
# <a name="introducing-multipoint-services"></a>Introduzione a Servizi MultiPoint
Il ruolo Servizi MultiPoint in Windows Server 2016 consente a più utenti, ognuno con un'esperienza Windows indipendente e familiare, di condividere simultaneamente un solo computer. Esistono diversi modi in cui gli utenti possono accedere alle proprie sessioni. Un modo consiste nel fare in modo che i servizi remoti vengano inseriti nel server usando le [app desktop remote](../remote-desktop-services/clients/remote-desktop-clients.md) con qualsiasi dispositivo. Un altro modo consiste nell'usare le stazioni fisiche collegate al server MultiPoint:  
  
-   Direttamente alle porte video del computer  
  
-   Tramite i client USB zero specializzati (detti anche hub USB multifunzione) e tramite dispositivi USB-over-Ethernet analoghi.  
  
-   Tramite la rete locale (LAN)  
  
Ognuno di questi metodi viene descritto più dettagliatamente nelle [stazioni MultiPoint Services](MultiPoint-services-Stations.md) più avanti in questo documento.  
  
Questo documento illustra i fattori seguenti da considerare quando si pianifica la distribuzione di servizi MultiPoint:  
  
-   Tipo di desktop da usare con il sistema MultiPoint Services: Ti servono sessioni, macchine virtuali o PC Windows?  
  
-   [Selezione dell'hardware per il sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md): Quali decisioni hardware è opportuno apportare?  
  
-   [Requisiti hardware e consigli sulle prestazioni](Hardware-Requirements-and-Performance-Recommendations.md): Quale hardware è necessario per servizi MultiPoint?  
  
-   [Pianificazione del sito di servizi multipoint](MultiPoint-services-Site-Planning.md): Dove si trovano i computer che eseguono MultiPoint Services e le rispettive stazioni e come verranno configurati?  
  
-   [Considerazioni sulla rete e account utente](Network-Considerations-and-User-Accounts.md): L'ambiente di rete in cui viene distribuito il sistema MultiPoint Services può influire sul modo in cui vengono gestiti gli account utente. Qual è l'ambiente di rete? Come vengono gestiti gli account utente?  
  
-   [Archiviazione di file con servizi multipoint](Storing-Files-with-MultiPoint-services.md): Dove verranno archiviati i file utente e come sarà possibile accedervi?  
  
-   [Elenco di controllo per la pre-distribuzione](Predeployment-Checklist.md)  