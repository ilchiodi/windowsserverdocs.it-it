---
title: Ripristino della foresta di Active Directory - Procedure
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: 0d427448c8d2a6616b87a524bcc941fc8555cbd4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390307"
---
# <a name="ad-forest-recovery---procedures"></a>Ripristino della foresta di Active Directory - Procedure

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

In questa sezione sono contenute le procedure relative al processo di ripristino della foresta. Le procedure sono applicabili a Windows Server 2016, 2012 R2, 2012 e sono applicabili anche a Windows Server 2008 R2 e 2008 con alcune eccezioni secondarie.

Le procedure che includono passaggi che variano per Windows Server 2003 sono disponibili nel [ripristino della foresta con i controller di dominio di Windows server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  

Di seguito è riportato un elenco di procedure utilizzate per eseguire il backup e il ripristino di controller di dominio e Active Directory.

- [Backup di un server completo](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [Backup dei dati sullo stato del sistema](AD-Forest-Recovery-Backing-up-System-State.md)  
- [Esecuzione di un ripristino completo del server](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [Esecuzione di una sincronizzazione autorevole di SYSVOL con replica DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [Esecuzione di un ripristino non autorevole di Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

Questa procedura illustra come eseguire un ripristino autorevole di SYSVOL nello stesso momento.  

- [Configurazione del servizio server DNS](AD-Forest-Recovery-Configure-DNS.md)  
- [Rimozione del catalogo globale](AD-Forest-Recovery-Remove-GC.md)  
- [Generazione del valore dei pool di RID disponibili](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [Invalidamento del pool di RID corrente](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [Requisizione di un ruolo di master operazioni](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [Pulizia dopo un ripristino](AD-Forest-Recovery-Cleanup.md)
- [Pulizia dei metadati di controller di dominio scrivibili rimossi](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [Reimpostazione della password dell'account computer del controller di dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [Reimpostazione della password KRBTGT](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [Reimpostazione di una password di trust su un lato della relazione di trust](AD-Forest-Recovery-Reset-Trust.md)  
- [Aggiunta del catalogo globale](AD-Forest-Recovery-Add-GC.md)  
- [Risorse per verificare il funzionamento della replica](AD-Forest-Recovery-Verify-Replication.md)  

## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta personalizzato](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta di Active Directory-identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta di Active Directory-determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta di Active Directory-esecuzione del ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta di Active Directory-Domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta di Active Directory-ripristino di un singolo dominio in una foresta con più domini](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta di Active Directory-ripristino della foresta con i controller di dominio Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
