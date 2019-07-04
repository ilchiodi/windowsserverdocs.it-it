---
title: Servizi Desktop remoto - Archiviazione sicura dei dati
description: Informazioni sulla pianificazione dell'archiviazione sicura dei dati usando dischi dei profili utente (UPD) in Servizi Desktop remoto.
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805164"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>Servizi Desktop remoto - Archiviazione sicura dei dati con UPD

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Archivia in modo sicuro risorse aziendali, dati di personalizzazione utente e impostazioni in locale o in Azure. Gli host sessione Desktop remoto usano l'autenticazione di AD e offrono agli utenti le risorse di cui hanno bisogno in un ambiente personalizzato, in modo sicuro. 

Garantire agli utenti un'esperienza coerente, indipendentemente dall'endpoint da cui accedono alle risorse remote, è un aspetto importante della gestione di una distribuzione di Servizi Desktop remoto. I dischi dei profili utente (UPD) consentono alle impostazioni di dati, personalizzazioni e applicazioni di seguire un utente all'interno di una singola raccolta. Un UPD è un file VHD con ambito raccolta e per utente salvato in una condivisione centrale montata in una sessione utente al momento dell'accesso: l'UPD viene considerato come un'unità locale per la durata di quella sessione. 

Dal punto di vista dell'utente, un UPD offre un'esperienza familiare. Gli utenti possono salvare i documenti nella cartella documenti (su quella che sembra un'unità locale), cambiare le impostazioni delle applicazioni come di consueto e apportare eventuali personalizzazioni al proprio ambiente di Windows. Tutti i dati, incluso l'hive del Registro di sistema, vengono archiviati sull'UPD e rimangono in una condivisione di rete centrale. Gli UPD sono disponibili per l'utente solo quando quest'ultimo è attivamente connesso a un desktop o RemoteApp. Gli UPD possono solo eseguire il roaming all'interno di una raccolta poiché l'intera directory utente `C:\Users\<username\>` (inclusi AppData\Local) è archiviata in un UPD.

È possibile usare i [cmdlet di PowerShell](https://technet.microsoft.com/library/jj215443.aspx) per definire il percorso della condivisione centrale, le dimensioni di ogni UPD e le cartelle da includere o escludere dal profilo utente salvato sull'UPD. In alternativa, è possibile abilitare gli UPD tramite Server Manager selezionando **Servizi Desktop remoto** > **Raccolte** > **Raccolta di desktop**  >  **Proprietà della raccolta di desktop** > **Dischi dei profili utente**. Si noti che un utente può abilitare o disabilitare gli UPD per tutti gli utenti di un'intera raccolta, non per utenti specifici di tale raccolta. Gli UPD devono essere archiviati in un file di condivisione centrale all'interno del quale i server della raccolta hanno il pieno controllo delle autorizzazioni. 

È possibile ottenere la disponibilità elevata per gli UPD archiviandoli in Azure con [spazi di archiviazione diretta](rds-storage-spaces-direct-deployment.md). 