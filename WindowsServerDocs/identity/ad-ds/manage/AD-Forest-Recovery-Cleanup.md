---
title: Ripristino della foresta Active Directory - pulizia
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 2f652d08304a17ecbfde51bbb6f35e4666cd9eca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---cleanup"></a>Ripristino della foresta Active Directory - pulizia 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 In base alle necessità, eseguire i passaggi di ripristino post seguenti:  
  
-   Dopo avere recuperato l'intera foresta, è possibile ripristinare la configurazione DNS originale, tra cui configurazione dei server DNS preferito e alternativo in ogni controller di dominio. Dopo che i server DNS sono configurati come si trovavano prima il malfunzionamento, verranno ripristinate le relative funzionalità di risoluzione nome precedente. Eliminare tutti i record DNS per i controller di dominio che non sono stati ripristinati.  
  
-   Eliminare i record di Windows Internet Name Service (WINS) per tutti i controller di dominio che non sono stati ripristinati.  
  
-   È possibile trasferire i ruoli di master operazioni in altri controller di dominio nel dominio o foresta e aggiungere i server di catalogo globale più in base alla configurazione prima dell'errore.  
  
-   Poiché l'intera foresta viene ripristinato a uno stato precedente, tutti gli oggetti (ad esempio, gli utenti e computer) che sono stati aggiunti e tutti gli aggiornamenti (ad esempio, le modifiche delle password) che sono stati apportati agli oggetti esistenti successivi a questo punto vengono persi. Pertanto, ricreare tali oggetti mancanti e riapplicare tutti gli aggiornamenti mancanti come appropriato.  
  
-   È anche potrebbe essere necessario ripristinare in uscita relazioni di trust con domini esterni e foreste, perché queste relazioni di trust esterno non vengono automaticamente ripristinate dal backup.

## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  



