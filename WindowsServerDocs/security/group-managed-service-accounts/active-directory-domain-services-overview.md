---
title: Panoramica di Active Directory Domain Services
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843132"
---
# <a name="active-directory-domain-services-overview"></a>Panoramica di Active Directory Domain Services

>Si applica a: Windows Server (canale semestrale), Windows Server 2016
  
Una directory è una struttura gerarchica che archivia le informazioni sugli oggetti sulla rete. Un servizio di directory, ad esempio Active Directory Domain Services (AD DS), fornisce i metodi per l'archiviazione dei dati di directory e renderli disponibili agli amministratori e gli utenti della rete. Ad esempio, Active Directory Domain Services archivia le informazioni sugli account utente, ad esempio nomi, password, i numeri di telefono e così via e consente ad altri utenti autorizzati nella stessa rete accedere a queste informazioni.  
  
Active Directory archivia le informazioni sugli oggetti sulla rete e più facile per gli amministratori e agli utenti di trovare e usare queste informazioni. Active Directory Usa un archivio dati strutturato come base per un'organizzazione logica e gerarchica di informazioni sulla directory.  
  
Questo archivio dati, noto anche come la directory contiene informazioni sugli oggetti di Active Directory. In genere, questi oggetti includono le risorse condivise, ad esempio i server, volumi, stampanti e gli account utente e computer di rete. Per altre informazioni sull'archivio dati di Active Directory, vedere [archivio dati di Directory](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).  
  
La protezione viene integrata con Active Directory tramite l'autenticazione di accesso e controllo di accesso agli oggetti nella directory. Con un unico accesso in rete, gli amministratori possono gestire i dati della directory e dell'organizzazione per tutta la rete e gli utenti autorizzati possono accedere alle risorse in un punto qualsiasi nella rete. L'amministrazione basata su criteri semplifica la gestione anche nel caso di reti di grande complessità. Per altre informazioni sulla sicurezza di Active Directory, vedere Cenni preliminari sulla sicurezza.  
  
Active Directory include anche:  
* Un set di regole **lo schema**, che definisce le classi di oggetti e attributi contenuti nella directory, i vincoli e i limiti per istanze di tali oggetti e il formato del nome. Per altre informazioni sullo schema, vedere lo Schema.  
  
  
* Oggetto **catalogo globale** che contiene informazioni su tutti gli oggetti nella directory. In questo modo gli utenti e amministratori per trovare le informazioni di directory indipendentemente dal fatto che di dominio nella directory contiene effettivamente i dati. Per altre informazioni sul catalogo globale, vedere il ruolo del catalogo globale.  
  
  
* Oggetto **meccanismo di query e indice**, in modo che gli oggetti e le relative proprietà possono essere pubblicate e rilevate da applicazioni o gli utenti della rete. Per altre informazioni sull'esecuzione di query di directory, vedere le informazioni della directory di ricerca.  
  
  
* Oggetto **servizio replica** che distribuisce i dati di directory tra una rete. Tutti i controller di dominio in un dominio fa parte della replica e contengono una copia completa di tutte le informazioni di directory per i loro domini. Qualunque modifica ai dati di directory viene replicata in tutti i controller di dominio del dominio. Per altre informazioni sulla replica di Active Directory, vedere Cenni preliminari sulla replica.  
  
## <a name="understanding-active-directory"></a>Informazioni su Active Directory  
 In questa sezione vengono forniti collegamenti a concetti di Active Directory:  
   
* [Struttura di Active Directory e le tecnologie di archiviazione](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)  
* [Ruoli di Controller di domini](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)   
* Schema di Active Directory   
* [Informazioni sui trust](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx)   
* [Tecnologie di replica di Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)   
* [Ricerca di Active Directory e le tecnologie di pubblicazione](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx)   
* Interoperabilità con DNS e criteri di gruppo   
* [Informazioni su schemi](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx)   
  
Per un elenco dettagliato dei concetti di Active Directory, vedere [informazioni su Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx).   

