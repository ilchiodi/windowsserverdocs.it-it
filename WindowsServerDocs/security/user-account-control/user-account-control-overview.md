---
title: Panoramica del controllo dell'account utente
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bdc9f4dc4b8e19d62288f12a4f2b4e8c86b93b68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403320"
---
# <a name="user-account-control-overview"></a>Panoramica del controllo dell'account utente
Controllo account utente \(\) UAC è un componente fondamentale della visione generale della sicurezza di Microsoft.  Il controllo dell'account utente consente di ridurre l'impatto di programmi dannosi.

## <a name="BKMK_OVER"></a>Descrizione della funzionalità
Controllo account utente consente a tutti gli utenti di accedere al proprio computer con un account utente standard. I processi avviati con un token utente standard possono eseguire le attività con i diritti di accesso concessi a un utente standard. Esplora risorse, ad esempio, eredita automaticamente le autorizzazioni a livello di utente standard. Inoltre, tutti i programmi eseguiti con Esplora risorse \(ad esempio, facendo doppio\-facendo clic su un collegamento all'applicazione\) eseguiti anche con il set di autorizzazioni utente standard. Molte applicazioni, incluse quelle incluse nel sistema operativo stesso, sono progettate per funzionare correttamente in questo modo.

Altre applicazioni, in particolare quelle che non sono state progettate specificamente con le impostazioni di sicurezza, spesso richiedono autorizzazioni aggiuntive per l'esecuzione corretta. Questi tipi di programmi sono detti applicazioni legacy. Inoltre, le azioni, ad esempio l'installazione di un nuovo software e la modifica della configurazione di programmi come Windows Firewall, richiedono più autorizzazioni rispetto a quelle disponibili per un account utente standard.

Quando un'applicazione deve essere eseguita con più di diritti utente standard, UAC può ripristinare i gruppi di utenti aggiuntivi nel token. Ciò consente all'utente di avere il controllo esplicito dei programmi che apportano modifiche a livello di sistema al computer o al dispositivo.

## <a name="BKMK_APP"></a>Applicazioni pratiche
La modalità Approvazione amministratore nel controllo dell'account utente consente di impedire l'installazione automatica di programmi dannosi senza la conoscenza di un amministratore. Consente inoltre di proteggere dal sistema involontario\-modifiche di ampio livello. Infine, questa modalità può essere usata per applicare un livello superiore di conformità nella quale gli amministratori devono fornire esplicitamente il consenso o le credenziali per ogni processo amministrativo.



