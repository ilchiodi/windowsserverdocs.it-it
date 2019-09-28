---
title: Concedere licenze CAL (Client Access License) per la distribuzione di Servizi Desktop remoto
description: Panoramica delle licenze CAL in Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 252ee776946ba0c387d7a6cdf3dc97ffdc55a591
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387583"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Concedere licenze CAL (Client Access License) per la distribuzione di Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Ogni utente e ogni dispositivo che si connette a un host sessione Desktop remoto necessita di una licenza CAL (Client Access License). Usare il servizio licenze Desktop remoto per installare, rilasciare e tenere traccia delle licenze CAL Servizi Desktop remoto.  

Quando un utente o un dispositivo si connette a un server Host sessione Desktop remoto, quest'ultimo determina se è necessaria una licenza CAL Servizi Desktop remoto. Il server Host sessione Desktop remoto richiede quindi una licenza CAL Servizi Desktop remoto al server licenze Desktop remoto. Se in un server licenze è disponibile una licenza CAL Servizi Desktop remoto appropriata, la licenza viene rilasciata al client per consentire la connessione al server Host sessione Desktop remoto e da lì al desktop o alle app che sta tentando di usare.

Sebbene sia previsto un periodo di prova delle licenze durante il quale non è richiesto alcun server licenze, alla scadenza di tale periodo i client devono disporre di una licenza CAL Servizi Desktop remoto valida e rilasciata da un server licenze prima di poter accedere a un server Host sessione Desktop remoto.

Usare le informazioni seguenti per apprendere il funzionamento delle licenze CAL di Servizi Desktop remoto e per distribuire e gestire le licenze:

- [Concedere licenze CAL (Client Access License) per la distribuzione di Servizi Desktop remoto](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [Informazioni sul modello CAL](#understanding-the-cals-model)
  - [Nota sulle versioni CAL](#note-about-cal-versions)

## <a name="understanding-the-cals-model"></a>Informazioni sul modello CAL

Sono disponibili due tipi di licenze CAL:

- Licenze CAL per dispositivo di Servizi Desktop remoto
- Licenze CAL per utente di Servizi Desktop remoto

Nella tabella seguente vengono descritte le differenze tra i due tipi di licenze CAL:

| Per dispositivo                                                     | Per utente                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Le licenze CAL vengono fisicamente assegnate a ogni dispositivo.                   | Le licenze CAL vengono assegnate a un utente di Active Directory.                                 |
| Le licenze CAL vengono rilevate dal server licenze.                        | Le licenze CAL vengono rilevate dal server licenze.                                          |
| Le licenze CAL possono essere rilevate indipendentemente dall'appartenenza ad Active Directory. | Le licenze CAL non possono essere rilevate all'interno di un gruppo di lavoro.                                       |
| È possibile revocare fino al 20% delle licenze CAL.                              | Non è possibile revocare le licenze CAL.                                                      |
| Le licenze CAL temporanee sono valide per 52-89 giorni.                       | Non sono disponibili licenze CAL temporanee.                                                |
| Le licenze CAL non possono essere sovrassegnate.                                  | Le licenze CAL possono essere sovrassegnate (in violazione del contratto di licenza Desktop remoto). |

Quando si usa il modello Per dispositivo, viene rilasciata una licenza temporanea la prima volta che un dispositivo si connette all'host sessione Desktop remoto. Quando il dispositivo si connette per la seconda volta il server licenze rilascerà una licenza CAL permanente per dispositivo di Servizi Desktop remoto, se il server licenze è attivo e se sono disponibili licenze CAL.

Quando si usa il modello Per utente, le licenze non vengono applicate e a ogni utente viene concessa una licenza per connettersi a un Host sessione Desktop remoto da qualsiasi dispositivo. Il server licenze rilascia le licenze da un pool di licenze CAL disponibili o da un pool di licenze CAL utilizzate più volte. È tuo compito assicurarti che tutti gli utenti dispongano di una licenza valida e che nessuna licenza CAL venga usata più volte del previsto; in caso contrario, vengono violate le condizioni di licenza di Servizi Desktop remoto.

Per assicurarsi di essere conformi alle condizioni di licenza di Servizi Desktop remoto, tenere traccia del numero di licenze CAL per utente di Servizi Desktop remoto usate nell'organizzazione e assicurarsi di disporre di un numero sufficiente di licenze CAL per utente installate nel server licenze per tutti gli utenti.

È possibile usare Gestione licenze Desktop remoto per tenere traccia e generare report sulle licenze CAL per utente di Servizi Desktop remoto.

## <a name="note-about-cal-versions"></a>Nota sulle versioni CAL

La licenza CAL usata da utenti o dispositivi deve corrispondere alla versione di Windows Server a cui si connette l'utente o il dispositivo. È possibile usare una licenza CAL precedente per accedere alle versioni più recenti di Windows Server, ma non è possibile usare una licenza CAL più recente per accedere alle versioni precedenti di Windows Server.

La tabella seguente mostra le licenze CAL compatibili con gli host sessione Desktop remoto e con gli host di virtualizzazione Desktop remoto.

|                  |Licenza CAL 2008 R2 e versioni precedenti|Licenza CAL 2012|Licenza CAL 2016|Licenza CAL 2019|
|---------------------------------|--------|--------|--------|--------|
| **Server licenze 2008, 2008 R2**| Sì    | No     | No     | No     |
| **Server licenze 2012**         | Sì    | Sì    | No     | No     |
| **Server licenze 2012 R2**      | Sì    | Sì    | No     | No     |
| **Server licenze 2016**         | Sì    | Sì    | Sì    | No     |
| **Server licenze 2019**         | Sì    | Sì    | Sì    | Sì    |

Qualsiasi server licenze Servizi Desktop remoto può ospitare licenze da tutte le versioni precedenti di Servizi Desktop remoto e la versione corrente di Servizi Desktop remoto. Ad esempio, un server licenze Servizi Desktop remoto di Windows Server 2016 può ospitare licenze da tutte le versioni precedenti di Servizi Desktop remoto, mentre un server licenze Servizi Desktop remoto di Windows Server 2012 R2 può ospitare solo licenze fino a Windows Server 2012 R2.
