---
title: Carichi di lavoro di Desktop remoto
description: Breve panoramica dei diversi tipi di carichi di lavoro per le macchine virtuali gestite da Desktop remoto.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/02/2019
ms.tgt_pltfrm: na
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 693d1abd91fbfa2d44a6af42dd9f5338bd461927
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919946"
---
# <a name="remote-desktop-workloads"></a>Carichi di lavoro di Desktop remoto

Gli utenti possono eseguire diversi tipi di carichi di lavoro nelle macchine virtuali gestite da Servizi Desktop remoto o Desktop virtuale Windows. Ridimensiona la distribuzione a seconda delle esigenze previste per ogni tipo di utente. La tabella seguente fornisce esempi di una gamma di tipi di carico di lavoro per aiutarti a stimare le dimensioni necessarie per le tue macchine virtuali. Dopo aver configurato le macchine virtuali, devi monitorarne continuamente l'utilizzo effettivo e adeguare di conseguenza le relative dimensioni. Se risulta che hai bisogno di una macchina virtuale più grande o più piccola, puoi aumentare o ridurre facilmente la distribuzione esistente in Azure.

La tabella seguente descrive ogni carico di lavoro. "Utenti di esempio" sono i tipi di utenti che potrebbero considerare più utile ciascun carico di lavoro. "App di esempio" sono i tipi di app che funzionano meglio per ciascun carico di lavoro.

| Tipo di carico di lavoro | Utenti di esempio | App di esempio |
| --- | --- | --- |
| Leggero | Utenti che svolgono attività di base di immissione di dati | Applicazioni di immissione in database, interfacce della riga di comando |
| Medio | Consulenti ed esperti in ricerche di mercato | Applicazioni di immissione in database, interfacce della riga di comando, Microsoft Word, pagine Web statiche |
| Pesante | Ingegneri informatici, creatori di contenuti | Applicazioni di immissione in database, interfacce della riga di comando, Microsoft Word, pagine Web statiche, Microsoft Outlook, Microsoft PowerPoint, pagine Web dinamiche |
| Potenza | Grafici, creatori di modelli 3D, ricercatori di Machine Learning | Applicazioni di immissione in database, interfacce della riga di comando, Microsoft Word, pagine Web statiche, Microsoft Outlook, Microsoft PowerPoint, pagine Web dinamiche, Adobe Photoshop, Adobe Illustrator, CAD, CAM |

Per informazioni sulle indicazioni relative al dimensionamento, vedi [Linee guida relative al dimensionamento delle macchine virtuali](virtual-machine-recs.md).
