---
title: Panoramica del controllo dell'account utente
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887672"
---
# <a name="user-account-control-overview"></a>Panoramica del controllo dell'account utente
Controllo dell'Account utente \(UAC\) è un componente fondamentale della visione complessiva della protezione di Microsoft.  Il controllo dell'account utente consente di ridurre l'impatto di programmi dannosi.

## <a name="BKMK_OVER"></a>Descrizione funzionalità
Controllo account utente consente a tutti gli utenti di accedere al proprio computer con un account utente standard. I processi avviati con un token utente standard possono eseguire le attività con i diritti di accesso concessi a un utente standard. Esplora risorse, ad esempio, eredita automaticamente le autorizzazioni a livello di utente standard. Inoltre, tutti i programmi che vengono eseguiti usando Windows Explorer \(ad esempio, fare doppio\-facendo clic su un collegamento dell'applicazione\) anche eseguire con il set delle autorizzazioni utente standard. Molte applicazioni, inclusi quelli che sono inclusi con il sistema operativo stesso, sono progettate per funzionare correttamente in questo modo.

Altre applicazioni, specialmente quelle che non sono stati progettati in particolare con le impostazioni di sicurezza in queste ragioni, spesso richiedono autorizzazioni aggiuntive per eseguire correttamente. Questi tipi di programmi sono detti applicazioni legacy. Inoltre, le azioni, ad esempio installa nuovo software e modifiche di configurazione per i programmi come Windows Firewall, richiedono più autorizzazioni rispetto a quanto disponibile a un account utente standard.

Quando un esigenze delle applicazioni per l'esecuzione con diritti utente standard oltre, controllo dell'account utente può ripristinare gruppi di utenti aggiuntivi al token. In questo modo l'utente abbia il controllo esplicito di programmi che apportano modifiche a livello di sistema per i propri computer o dispositivi.

## <a name="BKMK_APP"></a>Applicazioni pratiche
Modalità Approvazione amministratore in controllo dell'account utente consente di impedire l'installazione invisibile all'utente senza una conoscenza di un amministratore di programmi dannosi. Consente anche di proteggere da sistema accidentale\-modifiche wide. Infine, questa modalità può essere usata per applicare un livello superiore di conformità nella quale gli amministratori devono fornire esplicitamente il consenso o le credenziali per ogni processo amministrativo.



