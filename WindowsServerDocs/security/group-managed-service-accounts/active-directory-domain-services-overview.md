---
title: Panoramica di servizi di dominio Active Directory
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6cfe9479-5d17-41d5-939a-891e5233fdca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 3d0e849edbff3a481ffd28f83d7f14089030920d
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="active-directory-domain-services-overview"></a>Panoramica di servizi di dominio Active Directory

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016
  
Una directory è una struttura gerarchica che archivia le informazioni sugli oggetti in rete. Un servizio di directory, ad esempio servizi di dominio Active Directory (AD DS), fornisce i metodi per l'archiviazione dei dati di directory e renderli disponibili per gli amministratori e gli utenti della rete. Ad esempio, di dominio Active Directory archivia le informazioni sugli account utente, ad esempio nomi, password, numeri di telefono e così via e consente ad altri utenti autorizzati nella stessa rete accedere a queste informazioni.  
  
Active Directory archivia le informazioni sugli oggetti della rete e facile per gli amministratori e utenti di individuare e utilizzare queste informazioni. Active Directory utilizza un archivio dati strutturati come base per un'organizzazione logica e gerarchica delle informazioni di directory.  
  
Questo archivio dati, noto anche come la directory contiene informazioni sugli oggetti di Active Directory. In genere, questi oggetti includono le risorse condivise, ad esempio server, volumi, stampanti e gli account utente e computer di rete. Per ulteriori informazioni sull'archivio dati di Active Directory, vedere [archivio dati di Directory](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).  
  
Sicurezza è integrata con Active Directory tramite l'autenticazione di accesso e controllo di accesso a oggetti della directory. Con un unico accesso in rete, gli amministratori possono gestire dati di directory e dell'organizzazione per tutta la rete e gli utenti autorizzati possono accedere alle risorse in un punto qualsiasi nella rete. L'amministrazione basata su criteri semplifica la gestione di reti più complesse. Per ulteriori informazioni sulla protezione di Active Directory, vedere la panoramica di sicurezza.  
  
Active Directory include anche:  
* Un set di regole, **lo schema**, che definisce le classi di oggetti e gli attributi contenuti nella directory, i vincoli e i limiti per le istanze di questi oggetti e il formato dei nomi. Per ulteriori informazioni sullo schema, vedere lo Schema.  
  
  
* Un **catalogo globale** che contiene informazioni su ogni oggetto nella directory. In questo modo gli utenti e agli amministratori di trovare le informazioni di directory indipendentemente dal fatto di dominio nella directory contiene effettivamente i dati. Per ulteriori informazioni sul catalogo globale, vedere il ruolo del catalogo globale.  
  
  
* Un **meccanismo di query e indice**, in modo che gli oggetti e le relative proprietà possono essere pubblicata e disponibile per gli utenti di rete o applicazioni. Per ulteriori informazioni sull'esecuzione di query nella directory, vedere la ricerca delle informazioni di directory.  
  
  
* Un **servizio replica** che distribuisce i dati di directory in una rete. Tutti i controller di dominio in un dominio partecipano alla replica e contengono una copia completa di tutte le informazioni di directory per il rispettivo dominio. Eventuali modifiche ai dati di directory viene replicata in tutti i controller di dominio nel dominio. Per ulteriori informazioni sulla replica di Active Directory, vedere la panoramica di replica.  
  
## <a name="understanding-active-directory"></a>Informazioni di Active Directory  
 In questa sezione vengono forniti collegamenti a concetti di Active Directory:  
   
* [Struttura di Active Directory e tecnologie di archiviazione](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)  
* [Ruoli del Controller di domini](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)   
* Schema di Active Directory   
* [Informazioni sui trust](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx)   
* [Tecnologie di replica di Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)   
* [Ricerca in Active Directory e le tecnologie di pubblicazione](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx)   
* Interazione con DNS e criteri di gruppo   
* [Comprensione Schema](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx)   
  
Per un elenco dettagliato dei concetti di Active Directory, vedere [comprensione Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx).   
