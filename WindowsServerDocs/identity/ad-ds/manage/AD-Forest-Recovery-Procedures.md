---
title: Ripristino della foresta di Active Directory - Procedure
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: da45e3b20c370a2a37b0eab31a78216434dd60be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827722"
---
# <a name="ad-forest-recovery---procedures"></a>Ripristino della foresta di Active Directory - Procedure

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

In questa sezione contiene le procedure relative al processo di ripristino dell'insieme di strutture. Le procedure sono applicabili a Windows Server 2016, 2012 R2, 2012 e sono applicabili anche a Windows Server 2008 R2 e 2008 con alcune piccole eccezioni.

Le procedure che includono i passaggi che variano per Windows Server 2003 sono disponibili in [ripristino della foresta con controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  

Di seguito è riportato un elenco di procedure utilizzate nel backup e ripristino dei controller di dominio e Active Directory.

- [Backup di un server completo](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [Backup dei dati dello stato del sistema](AD-Forest-Recovery-Backing-up-System-State.md)  
- [Esecuzione di un ripristino completo del server](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [Eseguire una sincronizzazione autorevole di DFSR-replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [Esecuzione di un ripristino non autorevole di servizi di dominio Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

Questi passaggi illustrano come eseguire un ripristino autorevole di SYSVOL nello stesso momento.  

- [Configurazione del servizio Server DNS](AD-Forest-Recovery-Configure-DNS.md)  
- [Rimozione del catalogo globale](AD-Forest-Recovery-Remove-GC.md)  
- [Generare il valore disponibile di pool di RID](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [Invalidando il pool RID corrente](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [La requisizione un ruolo di master operazioni](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [Pulizia dopo un ripristino](AD-Forest-Recovery-Cleanup.md)
- [La pulizia dei metadati di controller di dominio scrivibili](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [Reimpostare la password dell'account computer del controller di dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [La reimpostazione della password krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [Reimpostare una password di attendibilità sul uno lato della relazione di trust](AD-Forest-Recovery-Reset-Trust.md)  
- [Aggiungere il catalogo globale](AD-Forest-Recovery-Add-GC.md)  
- [Risorse per verificare il funzionamento della replica](AD-Forest-Recovery-Verify-Replication.md)  

## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta Active Directory - concepire un piano di ripristino personalizzata-foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta Active Directory - il ripristino di un singolo dominio all'interno di un Multidomain foresta](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta Active Directory - ripristino della foresta con controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
