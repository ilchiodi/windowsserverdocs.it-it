---
title: Concedere licenze CAL (Client Access License) per la distribuzione di Servizi Desktop remoto
description: Panoramica delle licenze CAL in Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 02/12/2020
manager: dongill
ms.openlocfilehash: 295536afc77d0559fd7d2d4a22f555231a1aab75
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80858074"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Concedere licenze CAL (Client Access License) per la distribuzione di Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Ogni utente e ogni dispositivo che si connette a un host sessione Desktop remoto necessita di una licenza CAL (Client Access License). Usare il servizio licenze Desktop remoto per installare, rilasciare e tenere traccia delle licenze CAL Servizi Desktop remoto.  

Quando un utente o un dispositivo si connette a un server Host sessione Desktop remoto, quest'ultimo determina se è necessaria una licenza CAL Servizi Desktop remoto. Il server Host sessione Desktop remoto richiede quindi una licenza CAL Servizi Desktop remoto al server licenze Desktop remoto. Se in un server licenze è disponibile una licenza CAL Servizi Desktop remoto appropriata, la licenza viene rilasciata al client per consentire la connessione al server Host sessione Desktop remoto e da lì al desktop o alle app che sta tentando di usare.

Sebbene sia previsto un periodo di prova delle licenze durante il quale non è richiesto alcun server licenze, alla scadenza di tale periodo i client devono disporre di una licenza CAL Servizi Desktop remoto valida e rilasciata da un server licenze prima di poter accedere a un server Host sessione Desktop remoto.

Usare le informazioni seguenti per apprendere il funzionamento delle licenze CAL di Servizi Desktop remoto e per distribuire e gestire le licenze:

- [Concedere licenze CAL (Client Access License) per la distribuzione di Servizi Desktop remoto](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [Informazioni sul modello di licenze CAL Servizi Desktop remoto](#understanding-the-rds-cal-model)
  - [Compatibilità delle versioni delle licenze CAL Servizi Desktop remoto](#rds-cal-version-compatibility)

## <a name="understanding-the-rds-cal-model"></a>Informazioni sul modello di licenze CAL Servizi Desktop remoto

Sono disponibili due tipi di licenze Servizi Desktop remoto:

- Licenze CAL per dispositivo di Servizi Desktop remoto
- Licenze CAL per utente di Servizi Desktop remoto

Nella tabella seguente vengono descritte le differenze tra i due tipi di licenze CAL:

| Per dispositivo                                                     | Per utente                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Le licenze CAL Servizi Desktop remoto vengono fisicamente assegnate a ogni dispositivo.                   | Le licenze CAL Servizi Desktop remoto vengono assegnate a un utente in Active Directory.                                 |
| Le licenze CAL Servizi Desktop remoto vengono rilevate dal server licenze.                        | Le licenze CAL Servizi Desktop remoto vengono rilevate dal server licenze.                                          |
| Le licenze CAL Servizi Desktop remoto possono essere rilevate indipendentemente dall'appartenenza ad Active Directory. | Le licenze CAL Servizi Desktop remoto non possono essere rilevate all'interno di un gruppo di lavoro.                                       |
| Puoi revocare fino al 20% delle licenze CAL Servizi Desktop remoto.                              | Non puoi revocare le licenze CAL Servizi Desktop remoto.                                                      |
| Le licenze CAL Servizi Desktop remoto temporanee sono valide per 52-89 giorni.                       | Non sono disponibili licenze CAL Servizi Desktop remoto temporanee.                                                |
| Le licenze CAL Servizi Desktop remoto non possono essere sovrassegnate.                                  | Le licenze CAL Servizi Desktop remoto possono essere sovrassegnate (in violazione del contratto di licenza Desktop remoto). |

Quando si usa il modello Per dispositivo, viene rilasciata una licenza temporanea la prima volta che un dispositivo si connette all'host sessione Desktop remoto. Quando il dispositivo si connette per la seconda volta, il server licenze rilascia una licenza CAL Per dispositivo di Servizi Desktop remoto permanente, purché sia attivo e vi siano licenze CAL Servizi Desktop remoto disponibili.

Quando si usa il modello Per utente, le licenze non vengono applicate e a ogni utente viene concessa una licenza per connettersi a un Host sessione Desktop remoto da qualsiasi dispositivo. Il server licenze rilascia le licenze dal pool di licenze CAL Servizi Desktop remoto disponibili o dal pool di licenze CAL Servizi Desktop remoto usate più volte. È tuo compito assicurarti che tutti gli utenti dispongano di una licenza valida e che nessuna licenza CAL venga usata più volte del previsto; in caso contrario, vengono violate le condizioni di licenza di Servizi Desktop remoto.

Per essere conforme alle condizioni di licenza di Servizi Desktop remoto, tieni traccia del numero di licenze CAL Per utente di Servizi Desktop remoto usate nell'organizzazione e assicurati di disporre di un numero sufficiente di licenze CAL Per utente di Servizi Desktop remoto installate nel server licenze per tutti gli utenti.

È possibile usare Gestione licenze Desktop remoto per tenere traccia e generare report sulle licenze CAL per utente di Servizi Desktop remoto.

## <a name="rds-cal-version-compatibility"></a>Compatibilità delle versioni delle licenze CAL Servizi Desktop remoto

La licenza CAL Servizi Desktop remoto per gli utenti o i dispositivi deve essere compatibile con la versione di Windows Server a cui si connette l'utente o il dispositivo. Non puoi usare licenze CAL Servizi Desktop remoto di versioni precedenti per accedere a versioni di Windows Server successive, mentre puoi usare versioni successive delle licenze CAL Servizi Desktop remoto per accedere a versioni di Windows Server precedenti. Per la connessione a un host sessione Desktop remoto Windows Server 2016, ad esempio, è necessaria una licenza CAL Servizi Desktop remoto 2016 o versioni successive, mentre per la connessione a un host sessione Desktop remoto Windows Server 2012 R2 è necessaria una licenza CAL Servizi Desktop remoto 2012 o versioni successive.

Nella tabella seguente vengono illustrate le versioni di licenze CAL Servizi Desktop remoto e di host sessione Desktop remoto compatibili tra loro.

|                  | Licenza CAL Servizi Desktop remoto 2008 R2 e versioni precedenti | Licenza CAL Servizi Desktop remoto 2012 | Licenza CAL Servizi Desktop remoto 2016 | Licenza CAL Servizi Desktop remoto 2019 |
|---------------------------------|--------|--------|--------|--------|
| **Host sessione 2008, 2008 R2** | Sì    | Sì    | Sì    | Sì     |
| **Host sessione 2012**         | No     | Sì    | Sì    | Sì    |
| **Host sessione 2012 R2**      | No     | Sì    | Sì    | Sì    |
| **Host sessione 2016**         | No     | No     | Sì    | Sì    |
| **Host sessione 2019**         | No     | No     | No     | Sì    |

Devi installare la licenza CAL Servizi Desktop remoto in un server licenze Desktop remoto compatibile. Qualsiasi server licenze Servizi Desktop remoto può ospitare licenze da tutte le versioni precedenti di Servizi Desktop remoto e la versione corrente di Servizi Desktop remoto. Ad esempio, un server licenze Servizi Desktop remoto di Windows Server 2016 può ospitare licenze da tutte le versioni precedenti di Servizi Desktop remoto, mentre un server licenze Servizi Desktop remoto di Windows Server 2012 R2 può ospitare solo licenze fino a Windows Server 2012 R2.

Nella tabella seguente vengono illustrate le versioni di licenze CAL Servizi Desktop remoto e di server licenze compatibili tra loro.

|                  | Licenza CAL Servizi Desktop remoto 2008 R2 e versioni precedenti | Licenza CAL Servizi Desktop remoto 2012 | Licenza CAL Servizi Desktop remoto 2016 | Licenza CAL Servizi Desktop remoto 2019 |
|---------------------------------|--------|--------|--------|--------|
| **Server licenze 2008, 2008 R2** | Sì    | No   | No   | No    |
| **Server licenze 2012**         | Sì     | Sì    | No   | No    |
| **Server licenze 2012 R2**      | Sì     | Sì    | No   | No    |
| **Server licenze 2016**         | Sì     | Sì    | Sì   | No    |
| **Server licenze 2019**         | Sì     | Sì    | Sì  | Sì   |
