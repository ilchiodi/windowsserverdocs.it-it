---
title: Ripristino della foresta di Active Directory-aggiunta del GC
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: f82033dd042847c7c735423c25756b936b137230
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369344"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Ripristino della foresta di Active Directory-aggiunta del GC

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per aggiungere il catalogo globale a un controller di dominio.  
  
## <a name="to-add-the-global-catalog"></a>Per aggiungere il catalogo globale  
  
1. Fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **strumenti di amministrazione**, quindi fare clic su **Active Directory siti e servizi**.  
2. Nell'albero della console espandere il contenitore **siti** , quindi selezionare il sito appropriato che contiene il server di destinazione.  
3. Espandere il contenitore **Server** , quindi espandere l'oggetto server per il controller di dominio al quale si desidera aggiungere il catalogo globale.  
4. Fare clic con il pulsante destro del mouse su **Impostazioni NTDS**, quindi scegliere **Proprietà**.  
5. Selezionare la casella di controllo **catalogo globale** .  
![Aggiungi](media/AD-Forest-Recovery-Add-GC/addgc1.png) GC

## <a name="to-add-the-global-catalog-using-repadmin"></a>Per aggiungere il catalogo globale tramite repadmin  

- Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

Di seguito sono riportati i modi per velocizzare il processo di aggiunta del catalogo globale al controller di dominio nel dominio radice:  

- Idealmente, il controller di dominio nel dominio radice deve essere un partner di replica dei controller di dominio ripristinati nei domini non radice. In tal caso, verificare che il controllo di coerenza informazioni (KCC) abbia creato l'oggetto **repsFrom** corrispondente per il controller di dominio di origine e la partizione nel controller di dominio radice. È possibile confermare questa operazione eseguendo il comando **repadmin/showreps/v** . 

- Se non è stato creato alcun oggetto **repsFrom** , creare questo oggetto per la partizione di configurazione. In questo modo, il controller di dominio nel dominio radice può determinare quali controller di dominio nel dominio non radice sono stati eliminati. È possibile eseguire questa operazione con i comandi seguenti:  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   Il formato di *SourceDomainControllerCNAME* è:  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   Ad esempio, il comando repadmin/add per la partizione di configurazione del dominio contoso.com può essere:  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- Se l'oggetto **repsFrom** è presente, provare a sincronizzare il controller di dominio nel dominio radice con il controller di dominio nel dominio non radice, come indicato di seguito:  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   Dove *DestinationDomainController* è il controller di dominio nel dominio radice e *SOURCEDOMAINCONTROLLER* è il controller di dominio ripristinato nel dominio non radice. 

- Il server DNS del dominio radice deve avere i record di risorse alias (CNAME) per il controller di dominio di origine. Verificare che la zona DNS padre contenga i record di risorse di delega (server dei nomi (NS) e i record di risorse host (A) per i controller di dominio corretti (i controller di dominio ripristinati dal backup) nella zona figlio. 
- Verificare che il controller di dominio nel dominio radice stia contattando il Centro distribuzione chiavi corretto (KDC) nel dominio non radice. Per eseguire il test, al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
