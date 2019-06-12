---
title: 'Servizi Desktop remoto: archiviazione sicura dei dati'
description: Informazioni in modo sicuro l'archiviazione dei dati utilizzando dischi dei profili utente (UPD) in Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: c3c7be624e3b093347807a5ee131270d3c802f1a
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805164"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>Servizi Desktop remoto: archiviazione sicura dei dati con gli UPD

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016

Risorse di business Store, i dati sulla personalizzazione utente e impostazioni in modo sicuro in locale o in Azure. Host sessione Desktop remoto Usa l'autenticazione di AD e consentire agli utenti con le risorse che necessarie in un ambiente personalizzato, in modo sicuro. 

Garantire che gli utenti hanno un'esperienza coerente, indipendentemente dall'endpoint da cui accedere alle risorse remote, è un aspetto importante di gestione di una distribuzione di servizi desktop remoto. I dischi dei profili utente (UPD) consentono i dati dell'utente, le personalizzazioni e le impostazioni dell'applicazione per seguono un utente all'interno di una singola raccolta. Un UPD è un file di disco rigido virtuale per ogni raccolta salvato in una condivisione centrale montata in una sessione utente quando effettuano l'accesso - UPD viene considerato come un'unità locale per la durata della sessione per ogni utente. 

Dal punto di vista dell'utente, UPD offre un'esperienza famililar: salvare i documenti per i documenti in cartelle (su quello che sembra essere un'unità locale), modificare le impostazioni dell'app come di consueto e apportare le personalizzazioni al proprio ambiente di Windows. Tutti i dati, inclusi l'hive del Registro di sistema, vengono archiviati sull'UPD e rimane persistente in una condivisione di rete centrale. Gli UPD sono disponibili all'utente solo quando l'utente è attivamente connesso a un computer desktop o RemoteApp. Gli UPD possono solo effettuare il roaming all'interno di una raccolta poiché dell'intera utente `C:\Users\<username\>` directory (inclusi AppData\Local) è archiviato in UPD.

È possibile usare [i cmdlet di PowerShell](https://technet.microsoft.com/library/jj215443.aspx) per definire il percorso per la condivisione centrale, le dimensioni di ogni disco profili utente e le cartelle che devono da includere o escludere dal profilo utente salvato al UPD. In alternativa, è possibile abilitare gli UPD tramite Server Manager selezionando **Servizi Desktop remoto** > **raccolte** > **insiemi di Desktop**  >  **Delle proprietà di insieme di desktop** > **dischi dei profili utente**. Si noti che per attivare o disattivare gli UPD per tutti gli utenti di un'intera raccolta, non per utenti specifici in tale raccolta. Gli UPD devono essere archiviati in una condivisione file centrale in cui il server nell'insieme di disporre delle autorizzazioni controllo completo. 

È possibile ottenere la disponibilità elevata per gli UPD archiviandoli in Azure con un [spazi di archiviazione diretta](rds-storage-spaces-direct-deployment.md). 