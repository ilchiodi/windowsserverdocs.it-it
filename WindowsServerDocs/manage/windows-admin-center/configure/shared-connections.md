---
title: Configurare le connessioni condivise per tutti gli utenti del gateway Windows Admin Center
description: Scopri come gli amministratori possono configurare il gateway Windows Admin Center (Project Honolulu) una volta per consentire a tutti gli utenti di condividere un unico elenco di connessioni.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1bdafa0d23934666fc0f6985249502e4960dc38e
ms.sourcegitcommit: 74d9eee13386a039186a70cdc97dc0b82e74bbf4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/29/2019
ms.locfileid: "9271014"
---
# Configurare le connessioni condivise per tutti gli utenti del gateway Windows Admin Center

> Si applica a: Windows Admin Center Preview

Con la possibilità di configurare le connessioni condivise, gli amministratori di gateway possono configurare l'elenco di connessioni una sola volta per tutti gli utenti di un determinato gateway Windows Admin Center. 

Dalla scheda **Condivisa connessioni** di gateway Windows Admin Center impostazioni, gli amministratori di gateway possono aggiungere server, cluster e connessioni PC come faresti dalla pagina di tutte le connessioni, inclusa la possibilità di connessioni tag. Tutte le connessioni e i tag aggiunti all'elenco di connessioni condivisi verranno visualizzata per tutti gli utenti di questo gateway Windows Admin Center, dalla loro tutte le pagine di connessioni.
    ![](../media/shared-cnxns-1.png)

Quando gli utenti di Windows Admin Center accede alla pagina "Tutte le connessioni" dopo aver configurate le connessioni condivisi, vedranno le connessioni raggruppate in due sezioni: connessioni Shared e personali. Il gruppo personale elenco connessione dell'utente e vengono mantenuti in sessioni di tale utente del browser. Il gruppo di connessioni Shared è lo stesso per tutti gli utenti e non può essere modificato dalla pagina di tutte le connessioni.
![](../media/shared-cnxns-2.png)