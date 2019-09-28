---
title: Panoramica di Active Directory Domain Services
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c0ee85358c5e39f1ebf8cd901298a6083a3ebb97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403768"
---
# <a name="active-directory-domain-services-overview"></a>Panoramica di Active Directory Domain Services

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016
  
Una directory è una struttura gerarchica che archivia le informazioni sugli oggetti sulla rete. Un servizio directory, ad esempio Active Directory Domain Services (AD DS), fornisce i metodi per archiviare i dati della directory e renderli disponibili agli utenti e agli amministratori di rete. Ad esempio, servizi di dominio Active Directory archivia le informazioni sugli account utente, ad esempio nomi, password, numeri di telefono e così via, e consente ad altri utenti autorizzati della stessa rete di accedere a tali informazioni.  
  
Active Directory archivia le informazioni sugli oggetti sulla rete e semplifica la ricerca e l'utilizzo di queste informazioni da parte degli amministratori e degli utenti. Active Directory utilizza un archivio dati strutturato come base per un'organizzazione logica gerarchica delle informazioni di directory.  
  
Questo archivio dati, noto anche come directory, contiene informazioni sugli oggetti Active Directory. Questi oggetti includono in genere risorse condivise, ad esempio server, volumi, stampanti e account utente e computer di rete. Per altre informazioni sull'archivio dati Active Directory, vedere [archivio dati di directory](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).  
  
La sicurezza è integrata con Active Directory tramite l'autenticazione di accesso e il controllo di accesso agli oggetti nella directory. Con un unico accesso alla rete, gli amministratori possono gestire i dati e l'organizzazione della directory in tutta la rete e gli utenti di rete autorizzati possono accedere alle risorse in qualsiasi punto della rete. L'amministrazione basata su criteri semplifica la gestione anche nel caso di reti di grande complessità. Per ulteriori informazioni sulla sicurezza Active Directory, vedere Cenni preliminari sulla sicurezza.  
  
Active Directory include anche:  
* Un set di regole, ovvero **lo schema**, che definisce le classi degli oggetti e degli attributi contenuti nella directory, i vincoli e i limiti per le istanze di questi oggetti e il formato dei rispettivi nomi. Per ulteriori informazioni sullo schema, vedere Schema.  
  
  
* **Catalogo globale** contenente informazioni su tutti gli oggetti nella directory. In questo modo gli utenti e gli amministratori possono trovare le informazioni relative alla directory indipendentemente dal dominio nella directory che contiene effettivamente i dati. Per ulteriori informazioni sul catalogo globale, vedere il ruolo del catalogo globale.  
  
  
* Un **meccanismo di query e di indice**, in modo che gli oggetti e le relative proprietà possano essere pubblicati e trovati dagli utenti o dalle applicazioni di rete. Per ulteriori informazioni sull'esecuzione di query sulla directory, vedere ricerca di informazioni sulla directory.  
  
  
* **Servizio di replica** che distribuisce i dati di directory in rete. Tutti i controller di dominio in un dominio partecipano alla replica e contengono una copia completa di tutte le informazioni di directory per il rispettivo dominio. Qualunque modifica ai dati di directory viene replicata in tutti i controller di dominio del dominio. Per ulteriori informazioni sulla replica Active Directory, vedere Cenni preliminari sulla replica.  
  
## <a name="understanding-active-directory"></a>Informazioni Active Directory  
 In questa sezione vengono forniti i collegamenti ai concetti di base Active Directory:  
   
* [Struttura di Active Directory e tecnologie di archiviazione](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)  
* [Ruoli del controller di dominio](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)   
* Schema di Active Directory   
* [Informazioni sui trust](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx)   
* [Tecnologie di replica Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)   
* [Active Directory le tecnologie di ricerca e pubblicazione](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx)   
* Interoperabilità con DNS e Criteri di gruppo   
* [Informazioni sullo schema](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx)   
  
Per un elenco dettagliato dei concetti di Active Directory, vedere [informazioni sui Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx).   

