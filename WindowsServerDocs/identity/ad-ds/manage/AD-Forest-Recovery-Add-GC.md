---
title: Ripristino della foresta Active Directory - aggiunta di catalogo globale
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 156a4a64d9c3bb8261bd603b72ae11b81ff1d152
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825632"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Ripristino della foresta Active Directory - aggiunta di catalogo globale

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per aggiungere il catalogo globale a un controller di dominio.  
  
## <a name="to-add-the-global-catalog"></a>Per aggiungere il catalogo globale  
  
1. Fare clic su **avviare**, scegliere **tutti i programmi**, scegliere **strumenti di amministrazione**, quindi fare clic su **Active Directory Sites and Services**.  
2. Nell'albero della console, espandere la **siti** contenitore e quindi selezionare il sito appropriato che contiene il server di destinazione.  
3. Espandere la **server** contenitore, quindi espandere l'oggetto server per il controller di dominio a cui si desidera aggiungere il catalogo globale.  
4. Fare doppio clic su **impostazioni NTDS**, quindi fare clic su **proprietà**.  
5. Selezionare il **catalogo globale** casella di controllo.  
![Aggiungere GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>Per aggiungere il catalogo globale con Repadmin  

- Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

Di seguito sono modi per velocizzare il processo di aggiunta nel catalogo globale nel controller di dominio nel dominio radice:  

- In teoria, il controller di dominio nel dominio radice deve essere un partner di replica dei controller di dominio ripristinato nei domini non di primo livello. In questo caso, verificare che il controllo di coerenza informazioni (KCC) ha creato il corrispondente **repsFrom** oggetto per il controller di dominio di origine e la partizione nella radice del controller di dominio. È possibile verificarlo eseguendo il **repadmin /showreps /v** comando. 

- Se è presente alcun **repsFrom** oggetto creato, creare l'oggetto per la partizione di configurazione. In questo modo, il controller di dominio nel dominio radice può determinare quale controller di dominio nel dominio non radice sia stato eliminato. È possibile farlo con i comandi seguenti:  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   Il formato per il *SourceDomainControllerCNAME* è:  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   Ad esempio, il repadmin / aggiungere il comando per la partizione di configurazione del dominio contoso.com può essere:  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- Se il **repsFrom** oggetto è presente, provare a sincronizzare il controller di dominio nel dominio radice con il controller di dominio nel dominio non radice, come indicato di seguito:  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   In cui *DestinationDomainController* è il controller di dominio nel dominio radice e *SourceDomainController* è controller di dominio ripristinato nel dominio non radice. 

- Il server radice DNS di dominio deve avere i record di risorse alias (CNAME) per il controller di dominio di origine. Assicurarsi che la zona DNS padre contiene i record di risorsa di delega (server dei nomi (NS) e record di risorse host (A)) per i controller di dominio corretto (i controller di dominio che sono stati ripristinati da backup) nella zona figlio. 
- Assicurarsi che il controller di dominio nel dominio radice è contattando il corretto Centro distribuzione chiavi (KDC) nel dominio non radice. Per testarla, al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
