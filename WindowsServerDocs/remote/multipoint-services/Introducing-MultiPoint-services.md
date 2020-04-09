---
title: Introduzione a Servizi MultiPoint
description: Viene fornita una panoramica di MultiPoint Services, un modo per consentire a più utenti di condividere un sistema
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e547156bb46d7195baa64a0094f1d3ca432eb016
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859174"
---
# <a name="introducing-multipoint-services"></a>Introduzione a Servizi MultiPoint
Il ruolo Servizi MultiPoint in Windows Server 2016 consente a più utenti, ognuno con un'esperienza Windows indipendente e familiare, di condividere simultaneamente un solo computer. Esistono diversi modi in cui gli utenti possono accedere alle proprie sessioni. Un modo consiste nel fare in modo che i servizi remoti vengano inseriti nel server usando le [app desktop remote](../remote-desktop-services/clients/remote-desktop-clients.md) con qualsiasi dispositivo. Un altro modo consiste nell'usare le stazioni fisiche collegate al server MultiPoint:  
  
-   Direttamente alle porte video del computer  
  
-   Tramite i client USB zero specializzati (detti anche hub USB multifunzione) e tramite dispositivi USB-over-Ethernet analoghi.  
  
-   Tramite la rete locale (LAN)  
  
Ognuno di questi metodi viene descritto più dettagliatamente nelle [stazioni MultiPoint Services](MultiPoint-services-Stations.md) più avanti in questo documento.  
  
Questo documento illustra i fattori seguenti da considerare quando si pianifica la distribuzione di servizi MultiPoint:  
  
-   Quali tipi di desktop usare con il sistema MultiPoint Services: sono necessari sessioni, macchine virtuali o PC Windows?  
  
-   [Selezione dell'hardware per il sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md): quali decisioni hardware è necessario apportare?  
  
-   [Requisiti hardware e consigli sulle prestazioni](Hardware-Requirements-and-Performance-Recommendations.md): quale hardware è necessario per servizi multipoint?  
  
-   [Pianificazione del sito di servizi multipoint](MultiPoint-services-Site-Planning.md): dove verranno individuati i computer che eseguono multipoint Services e le rispettive stazioni e come verranno configurati?  
  
-   [Considerazioni sulla rete e account utente](Network-Considerations-and-User-Accounts.md): l'ambiente di rete in cui viene distribuito il sistema MultiPoint Services può influire sulla modalità di gestione degli account utente. Qual è l'ambiente di rete? Come vengono gestiti gli account utente?  
  
-   [Archiviazione di file con servizi multipoint](Storing-Files-with-MultiPoint-services.md): dove verranno archiviati i file utente e come verrà eseguito l'accesso?  
  
-   [Elenco di controllo per la pre-distribuzione](Predeployment-Checklist.md)  