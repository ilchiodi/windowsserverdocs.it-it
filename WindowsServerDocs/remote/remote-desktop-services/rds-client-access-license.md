---
title: La distribuzione di servizi desktop remoto di licenza con licenze di accesso client (CAL)
description: Panoramica del client di gestione delle licenze in Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 09/20/2018
manager: dongill
ms.openlocfilehash: 6648a52bb4d09725935a2197d6ce6fa6d8cc74a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853442"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>La distribuzione di servizi desktop remoto di licenza con licenze di accesso client (CAL)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Ogni utente e dispositivo che si connette a un host sessione Desktop remoto richiede un client access License (CAL). Utilizzare Servizio licenze Desktop remoto per installare, rilasciare e tenere traccia delle licenze CAL Servizi Desktop remoto.  

Quando un utente o un dispositivo si connette a un server Host sessione Desktop remoto, il server Host sessione Desktop remoto determina se è necessaria una CAL RDS. Il server Host sessione Desktop remoto invia quindi richieste una CAL Servizi Desktop remoto dal server licenze di Desktop remoto. Se una licenza CAL Servizi Desktop remoto appropriato è disponibile da un server licenze, le CAL RDS viene rilasciato al client e il client è in grado di connettersi al server Host sessione Desktop remoto e da lì sul desktop o le app che stai provando a usare.

Anche se non esiste un periodo di prova durante il quale Nessuna licenza server è necessario, dopo il periodo di tolleranza periodo i client debba avere una licenza CAL Servizi Desktop remoto valido emesso da un server licenze prima di essi può accedere a un server Host sessione Desktop remoto.

Usare le informazioni seguenti per informazioni sull'uso di licenze CAL di Servizi Desktop remoto e per distribuire e gestire le licenze:

- [Comprendere il modello CAL Servizi Desktop remoto](#understanding-the-cals-model)
- [Attivare il server licenze](rds-activate-license-server.md)
- [Installare le licenze CAL Servizi Desktop remoto nel server licenze](rds-install-cals.md)
- [Tenere traccia delle licenze CAL usate nella distribuzione](rds-track-cals.md)

## <a name="understanding-the-cals-model"></a>Informazioni sul modello di licenze CAL

Esistono due tipi di licenze CAL:

- Servizi Desktop remoto licenze CAL Per dispositivo
- Licenze CAL utente Per

Nella tabella seguente vengono descritte le differenze tra i due tipi di licenze CAL:

| Per dispositivo                                                     | Per utente                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Le licenze CAL sono fisicamente assegnata a ogni dispositivo.                   | CAL Servizi Desktop remoto viene assegnate a un utente di Active Directory.                                 |
| Le licenze CAL vengono rilevate dal server licenze.                        | Le licenze CAL vengono rilevate dal server licenze.                                          |
| È possibile tenere le licenze CAL indipendentemente dall'appartenenza ad Active Directory. | Le licenze CAL non possono essere rilevate all'interno di un gruppo di lavoro.                                       |
| È possibile revocare fino al 20% delle licenze CAL.                              | Non è possibile revocare le licenze CAL.                                                      |
| Le licenze CAL temporanea sono valide per 52 – 89 giorni.                       | Non sono disponibili licenze CAL temporanea.                                                |
| Le licenze CAL non può essere sovrassegnata.                                  | Le licenze CAL possono essere sovrallocati (in violazione del contratto di licenza Desktop remoto). |

Quando si usa il modello per ogni dispositivo, una licenza temporanea viene generata la prima volta che un dispositivo si connette all'Host sessione Desktop remoto. La seconda volta che il dispositivo si connette, purché il server licenze viene attivato e sono disponibili licenze di accesso client, il server licenze rilascerà una licenza CAL Per dispositivo permanente.

Quando si usa il modello per ogni utente, gestione delle licenze non viene applicata e ogni utente viene concessa una licenza per connettersi a un Host sessione Desktop remoto da un numero qualsiasi di dispositivi. Il server licenze emette le licenze da pool CAL disponibile o il pool Over-Used CAL. È compito del programmatore garantire che tutti gli utenti hanno una licenza valida e zero Over-Used CAL Servizi Desktop remoto, in caso contrario, ci si trova in violazione delle condizioni di licenza di Servizi Desktop remoto.

Per assicurarsi che sia in conformità con le condizioni di licenza di servizi desktop remoto, tenere traccia del numero di CAL Per utente usate nell'organizzazione e assicurarsi di avere un numero sufficiente licenze CAL Per utente installato nel server licenze per tutti gli utenti.

È possibile utilizzare Gestione licenze Desktop remoto per rilevare e generare rapporti su CAL Per utente.

## <a name="note-about-cal-versions"></a>Nota sulle versioni CAL

La licenza CAL usate da utenti o dispositivi deve corrispondere alla versione di Windows Server che l'utente o il dispositivo si connette a. È possibile usare le licenze CAL precedente per accedere alle versioni più recenti di Windows Server, ma è possibile usare le licenze CAL più recente per accedere alle versioni precedenti di Windows Server.

La tabella seguente mostra le licenze CAL compatibili presenti negli host sessione Desktop remoto e gli host di virtualizzazione Desktop remoto.

|                  |2008 R2 e versioni precedenti CAL|LICENZA CAL 2012|LICENZA CAL 2016|LICENZA CAL 2019|
|---------------------------------|--------|--------|--------|--------|
| **2008, 2008 R2 server licenze**| Yes    | No     | No     | No     |
| **server licenze 2012**         | Yes    | Yes    | No     | No     |
| **Server licenze 2012 R2**      | Yes    | Yes    | No     | No     |
| **server licenze 2016**         | Yes    | Yes    | Yes    | No     |
| **server licenze 2019**         | Yes    | Yes    | Yes    | Yes    |

Qualsiasi server licenze di servizi desktop remoto può ospitare le licenze da tutte le versioni precedenti di Servizi Desktop remoto e la versione corrente di Servizi Desktop remoto. Ad esempio, un server licenze di servizi desktop remoto di Windows Server 2016 può ospitare le licenze di tutte le versioni precedenti di servizi desktop remoto, mentre un server licenze di Windows Server 2012 R2 Servizi Desktop remoto può ospitare solo licenze fino a Windows Server 2012 R2.
