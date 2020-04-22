---
title: Configurare connessioni condivise per tutti gli utenti del gateway Windows Admin Center
description: Scopri la procedura che consente agli amministratori di configurare una sola volta il gateway Windows Admin Center (Project Honolulu) per consentire a tutti gli utenti di condividere un unico elenco di connessioni.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 943830a2743f7cfd3192a474eb36d57f734d3d34
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269308"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configurare connessioni condivise per tutti gli utenti del gateway Windows Admin Center

> Si applica a: Windows Admin Center Preview, Windows Admin Center

Con la possibilità di configurare connessioni condivise, gli amministratori del gateway possono configurare l'elenco delle connessioni una sola volta per tutti gli utenti di un determinato gateway Windows Admin Center. Questa funzionalità è disponibile solo in modalità servizio di Windows Admin Center.

Nella scheda **Shared Connections** (Connessioni condivise) di Impostazioni del gateway Windows Admin Center gli amministratori del gateway possono aggiungere server, cluster e connessioni PC come nella pagina Tutte le connessioni, sfruttando anche della possibilità di contrassegnare tali connessioni con tag. Tutti i tag e le connessioni aggiunti nell'elenco Shared Connections (Connessioni condivise) verranno visualizzati per tutti gli utenti di questo gateway Windows Admin Center nella rispettiva pagina Tutte le connessioni.
    ![](../media/shared-cnxns-1.png)

Quando un utente di Windows Admin Center accede alla pagina "Tutte le connessioni" dopo aver configurato Shared Connections (Connessioni condivise), visualizzerà le connessioni raggruppate in due sezioni: connessioni personali e condivise. Il gruppo di connessioni personali include l'elenco di connessioni di un utente specifico e permane nelle sessioni del browser dell'utente. Il gruppo delle connessioni condivise è identico per tutti gli utenti e non può essere modificato nella pagina Tutte le connessioni.
![](../media/shared-cnxns-2.png)