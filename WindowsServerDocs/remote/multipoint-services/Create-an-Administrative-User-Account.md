---
title: Creare un account utente con privilegi amministrativi
description: Creare un account con privilegi amministrativi in MultiPoint Services
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ce4c5a9-3dec-412f-910b-54a252f8f209
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 6737f7b96396a13aa18485095e0687425cf8b93e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389696"
---
# <a name="create-an-administrative-user-account"></a>Creare un account utente con privilegi amministrativi
Creare *account utente con privilegi amministrativi* per coloro che gestiranno il sistema MultiPoint Services. Per vedere quali utenti hanno accesso amministrativo, nella console di gestione MultiPoint, fare clic su di **utenti** scheda. Gli account utente con privilegi amministrativi sono visualizzati nella colonna Account Type (Tipo di account) come **Administrator** (Amministratore). *Gli utenti amministrativi* hanno accesso a tutte le attività di gestione MultiPoint che modificare le impostazioni di desktop e di sistema, ad esempio:  
  
-   Creazione di account  
  
-   Aggiunta e rimozione di programmi  
  
-   Gestione di *desktop* e hardware  
  
-   Chiusura di altre *sessioni* utente  
  
Gli utenti con privilegi amministrativi possono eseguire attività che riguardano tutti gli altri utenti del sistema MultiPoint Services, ad esempio installazione di software o modifica delle impostazioni di protezione. Per questa ragione, gli utenti con privilegi amministrativi devono avere password e nomi utente univoci noti solo a loro.  
  
Per altre informazioni sui problemi che gli utenti con privilegi amministrativi devono tenere in considerazione per la creazione e la gestione di account utente, vedere l'argomento [Considerazioni sull'account utente](User-Account-Considerations.md).  
  
> [!NOTE]  
> L'utente può creare un *account utente standard* da usare quando si eseguono attività sul sistema MultiPoint Services che non riguardano la gestione del sistema MultiPoint Services. In quel caso l'utente deve accedere all'account utente con privilegi amministrativi solo quando deve eseguire attività di gestione del sistema.  
  
#### <a name="to-create-an-administrative-user-account"></a>Per creare un account utente con privilegi amministrativi  
  
1.  Selezionare Gestione MultiPoint il **utenti** scheda.  
  
2.  In **User Tasks** (Attività utente) fare clic su **Add user account** (Aggiungi account utente). Verrà visualizzata la procedura guidata **Add User Account** (Aggiungi account utente).  
  
3.  Nel campo **User account** (Account utente) digitare un nome di accesso per l'utente. Il nome utente di accesso di solito è costituito da nome e cognome senza spazi intermedi oppure dall'iniziale del nome e dal cognome senza spazi intermedi.  
  
4.  Nel campo **Full name** (Nome completo) digitare il nome dell'utente nel formato preferito, ad esempio nome, nome completo o soprannome.  
  
5.  Nel campo **Password** digitare una password per l'utente. La password deve essere nota solo all'amministratore e all'utente e deve essere conservata in un luogo sicuro. La password può essere modificata solo da un utente con privilegi amministrativi.  
  
6.  Nel campo **Confirm password** (Conferma password) digitare nuovamente la password e fare clic su **Next** (Avanti).  
  
7.  Nella pagina di impostazione del livello di accesso selezionare **Administrative user** (Utente con privilegi amministrativi) e quindi fare clic su **Next** (Avanti).  
  
8.  MultiPoint Services verificherà tutte le informazioni e visualizzerà un messaggio quando l'account sarà stato impostato. Quando viene visualizzato il testo **A new user account was successfully created** (Nuovo account utente creato), fare clic su **Finish** (Fine).  
