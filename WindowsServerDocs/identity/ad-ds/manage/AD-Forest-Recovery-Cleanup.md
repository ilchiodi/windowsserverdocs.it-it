---
title: Ripristino della foresta Active Directory - pulizia
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fa7193cc800eac0fee6425a66bd5cd82d8c822c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868962"
---
# <a name="ad-forest-recovery---cleanup"></a>Ripristino della foresta Active Directory - pulizia

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Eseguire la procedura di ripristino post seguente in base alle esigenze:  
  
- Dopo avere recuperato l'intera foresta, è possibile ripristinare la configurazione DNS originale, inclusa la configurazione dei server DNS preferito e alternativo in ognuno dei controller di dominio. Dopo che i server DNS sono configurati come si presentavano prima il malfunzionamento, verranno ripristinate le relative funzionalità di risoluzione nome precedente. Eliminare tutti i record DNS per i controller di dominio che non sono stati ripristinati.  
- Eliminare i record di Windows Internet Name Service (WINS) per tutti i controller di dominio che non sono stati ripristinati.  
- È possibile trasferire i ruoli di master operazioni controller di dominio nel dominio o foresta e aggiungere server di catalogo globale più in base alla configurazione prima dell'errore.  
- Poiché l'intera foresta viene ripristinato in uno stato precedente, tutti gli oggetti (ad esempio utenti e computer) che sono state aggiunte e tutti gli aggiornamenti (ad esempio, le modifiche delle password) che sono stato apportati agli oggetti esistenti dopo questo punto vengono persi. Pertanto, si devono ricreare questi oggetti mancanti e riapplicare gli aggiornamenti mancanti come appropriato.  
- È inoltre potrebbe essere necessario ripristinare il trust in uscita con domini esterni e foreste, perché queste relazioni di trust esterno non vengono ripristinate automaticamente dai backup.

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
