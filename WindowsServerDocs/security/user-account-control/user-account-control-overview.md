---
title: Panoramica del controllo Account utente
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7a39cd-fc10-4408-befd-4b2c45806732
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 90ce72cb3d1850563d16a12d09a6872d107c0690
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="user-account-control-overview"></a>Panoramica del controllo Account utente
\(UAC\) controllo dell'Account utente è un componente fondamentale della visione generale sulla sicurezza di Microsoft.  Controllo dell'account utente consente di ridurre l'impatto di programmi dannosi.

## <a name="BKMK_OVER"></a>Descrizione delle funzionalità
Controllo dell'account utente consente a tutti gli utenti di accedere al proprio computer con un account utente standard. I processi avviati con un token utente standard possono eseguire le attività con diritti di accesso concessi a un utente standard. In Esplora risorse, ad esempio, eredita automaticamente le autorizzazioni a livello di utente standard. Inoltre, tutti i programmi che vengono eseguiti mediante Windows Explorer \ (ad esempio, facendo doppio shortcut\ un'applicazione) eseguire anche con il set di autorizzazioni utente standard. Molte applicazioni, inclusi quelli che sono inclusi con il sistema operativo stesso, sono progettate per funzionare correttamente in questo modo.

Altre applicazioni, in particolare quelli che non sono stati progettati con le impostazioni di sicurezza presente, spesso richiedono autorizzazioni aggiuntive per funzionare correttamente. Questi tipi di programmi vengono definiti come applicazioni legacy. Inoltre, azioni come l'installazione di nuovo software e modifiche di configurazione per i programmi, ad esempio Windows Firewall richiedono autorizzazioni maggiori di ciò che è disponibile per un account utente standard.

Quando un esigenze di applicazioni per l'esecuzione con diritti utente più standard, controllo dell'account utente può ripristinare gruppi di utenti aggiuntivi al token. In questo modo all'utente di controllare esplicitamente i programmi che apportano modifiche a livello di sistema per i computer o dispositivo.

## <a name="BKMK_APP"></a>Applicazioni pratiche
Modalità Approvazione amministratore in controllo dell'account utente consente di impedire l'installazione invisibile all'utente insaputa dell'amministratore di programmi dannosi. Consente inoltre di proteggere da modifiche accidentali a livello di trascrizione. Infine, può essere utilizzato per applicare un livello superiore di conformità in cui gli amministratori devono attivamente consenso o fornire le credenziali per ogni processo amministrativo.



