---
title: Configurare le connessioni condivise per tutti gli utenti del gateway dell'interfaccia di amministrazione di Windows
description: Informazioni su come gli amministratori possono configurare il gateway dell'interfaccia di amministrazione di Windows (Project Honolulu) una volta per consentire a tutti gli utenti di condividere un unico elenco di connessioni.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c632c178e91b92e0a80d8c72e8ea0ce3ce37b502
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357292"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configurare le connessioni condivise per tutti gli utenti del gateway dell'interfaccia di amministrazione di Windows

> Si applica a: Windows Admin Center Preview, interfaccia di amministrazione di Windows

Con la possibilità di configurare connessioni condivise, gli amministratori del gateway possono configurare l'elenco delle connessioni una volta per tutti gli utenti di un gateway dell'interfaccia di amministrazione di Windows specifico. 

Dalla scheda **connessioni condivise** delle impostazioni del gateway dell'interfaccia di amministrazione di Windows, gli amministratori del gateway possono aggiungere server, cluster e connessioni PC come si farebbe dalla pagina tutte le connessioni, inclusa la possibilità di contrassegnare le connessioni. Tutte le connessioni e i tag aggiunti nell'elenco connessioni condivise verranno visualizzati per tutti gli utenti del gateway dell'interfaccia di amministrazione di Windows, dalla pagina tutte le connessioni.
    ![](../media/shared-cnxns-1.png)

Quando un utente dell'interfaccia di amministrazione di Windows accede alla pagina "tutte le connessioni" dopo aver configurato le connessioni condivise, visualizzerà le connessioni raggruppate in due sezioni: Connessioni personali e condivise. Il gruppo personale è l'elenco di connessioni di un utente specifico e viene reso permanente nelle sessioni del browser dell'utente. Il gruppo di connessioni condivise è lo stesso in tutti gli utenti e non può essere modificato dalla pagina tutte le connessioni.
![](../media/shared-cnxns-2.png)