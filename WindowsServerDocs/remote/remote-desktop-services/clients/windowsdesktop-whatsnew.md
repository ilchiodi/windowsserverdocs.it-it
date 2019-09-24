---
title: Novità del client desktop di Windows
description: Informazioni sulle modifiche recenti apportate al client Desktop remoto per Windows Desktop
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: 587ad44509451497b253689238c3aef233a37153
ms.sourcegitcommit: e210fce039452a9855353520c7f8698acd76ffce
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2019
ms.locfileid: "71071288"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novità del client desktop di Windows

Per informazioni più dettagliate sul client desktop di Windows, vedi [Introduzione al client desktop di Windows](windowsdesktop.md). Di seguito sono riportati gli aggiornamenti del client più recenti.

## <a name="latest-client-versions"></a>Versioni del client più recenti

Il client può essere configurato per [gruppi di utenti](windowsdesktop-admin.md#configure-user-groups) diversi. Nella tabella seguente sono elencate le versioni correnti disponibili per ogni gruppo di utenti:

|Gruppo di utenti |Versione  |
|-----------|---------|
|Public     |1.2.246  |
|Insider    |1.2.247  |

## <a name="updates-for-version-12247"></a>Aggiornamenti per la versione 1.2.247

*Data di pubblicazione: 17/09/2019*

- È stato risolto un problema di arresto anomalo del sistema che si verificava al momento dell'autenticazione durante una connessione.
- È stato risolto un problema di arresto anomalo del sistema che si verificava durante la chiusura del client.

## <a name="updates-for-version-12246"></a>Aggiornamenti per la versione 1.2.246

*Data di pubblicazione: 28/08/2019*

- Sono state migliorate le lingue di fallback per la versione localizzata. Ad esempio, FR-CA verrà visualizzato correttamente in francese anziché in inglese.
- Quando viene rimossa una sottoscrizione, il client ora rimuove correttamente da Gestione credenziali le credenziali salvate.
- Dopo l'avvio, il processo di aggiornamento del client ora è automatico e, al termine, il client viene riavviato.
- Il client ora può essere usato in Windows 10 in modalità S.
- È stato risolto un problema a causa del quale il processo di aggiornamento aveva esito negativo per gli utenti con uno spazio nel nome utente.
