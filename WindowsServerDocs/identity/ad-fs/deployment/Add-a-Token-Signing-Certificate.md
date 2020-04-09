---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Aggiungere un certificato per la firma di token
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9b737cf8c9efb89ef9b3befaa1875b273bfcadf9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814934"
---
# <a name="add-a-token-signing-certificate"></a>Aggiungere un certificato per la firma di token


I server federativi in Active Directory Federation Services \(AD FS\) richiedono token\-certificati di firma per impedire agli utenti malintenzionati di modificare o contraffare i token di sicurezza nel tentativo di ottenere accesso non autorizzato alle risorse federate. Ogni token\-certificato di firma contiene chiavi private di crittografia e le chiavi pubbliche utilizzate per firmare digitalmente \(tramite la chiave privata\) un token di sicurezza. Successivamente, dopo che queste chiavi sono state ricevute da un server federativo partner, convalidano l'autenticità \(per mezzo della chiave pubblica\) del token di sicurezza crittografato.  
  
> [!CAUTION]  
> I certificati utilizzati per la firma di token\-sono fondamentali per la stabilità del Servizio federativo. Poiché la perdita o la rimozione non pianificata di tutti i certificati configurati per questo scopo può causare l'interruzione del servizio, è necessario eseguire il backup di tutti i certificati configurati  
  
Il token\-certificato di firma deve essere concatenato a una radice attendibile nel Servizio federativo. È possibile utilizzare la procedura seguente per aggiungere il token\-certificato di firma allo snap di gestione AD FS\-in da un file esportato.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Per aggiungere un token\-certificato di firma  
  
1.  Nella schermata **Start** Digitare**ad FS Management**, quindi premere INVIO.  
  
2.  Nell'albero della console fare doppio\-fare clic su **servizio**, quindi fare clic su **certificati**.  
  
3.  Nel riquadro **azioni** fare clic sul collegamento **Aggiungi token\-firma del certificato** .  
  
4.  Nella finestra di dialogo **Cerca file di certificato** passare al file del certificato che si desidera aggiungere, selezionare il file del certificato e quindi fare clic su **Apri**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

