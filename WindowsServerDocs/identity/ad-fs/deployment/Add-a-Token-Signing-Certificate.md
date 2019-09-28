---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Aggiungere un certificato per la firma di token
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c8b2246842dd70c06442faed995f6b883dbaf70a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360091"
---
# <a name="add-a-token-signing-certificate"></a>Aggiungere un certificato per la firma di token


I server federativi in Active Directory Federation Services \(AD FS @ no__t-1 richiedono il token @ no__t-2signing certificati per impedire agli utenti malintenzionati di modificare o contraffare i token di sicurezza nel tentativo di ottenere accesso non autorizzato a federato risorse. Ogni token\-certificato di firma contiene chiavi private di crittografia e le chiavi pubbliche utilizzate per firmare digitalmente \(tramite la chiave privata\) un token di sicurezza. Successivamente, dopo che queste chiavi sono state ricevute da un server federativo partner, convalidano l'autenticità \(BY significa della chiave pubblica @ no__t-1 del token di sicurezza crittografato.  
  
> [!CAUTION]  
> I certificati usati per il token @ no__t-0signing sono fondamentali per la stabilità del Servizio federativo. Poiché la perdita o la rimozione non pianificata di tutti i certificati configurati per questo scopo può causare l'interruzione del servizio, è necessario eseguire il backup di tutti i certificati configurati  
  
Il token @ no__t-0signing certificate deve essere concatenato a una radice attendibile nel Servizio federativo. È possibile utilizzare la procedura seguente per aggiungere il certificato @ no__t-0signing del token allo snap-in di gestione AD FS @ no__t-1in da un file esportato.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere \( [gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) di\/dominio http:\/\/go.Microsoft.com\/fwlink? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Per aggiungere un token @ no__t-0signing certificate  
  
1.  Nella schermata **Start** Digitare**ad FS Management**, quindi premere INVIO.  
  
2.  Nell'albero della console, Double @ no__t-0click **Service**, quindi fare clic su **Certificates**.  
  
3.  Nel riquadro **azioni** fare clic sul collegamento **Aggiungi token @ No__t-2Signing certificate** .  
  
4.  Nella finestra di dialogo **Cerca file di certificato** passare al file del certificato che si desidera aggiungere, selezionare il file del certificato e quindi fare clic su **Apri**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

