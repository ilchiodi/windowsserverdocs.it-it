---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Aggiungere un certificato per la firma di token
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ac9f9b95ad6226a8e3b7012e317899f1d48c60c9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192473"
---
# <a name="add-a-token-signing-certificate"></a>Aggiungere un certificato per la firma di token


Server federativo in Active Directory Federation Services \(ADFS\) richiedono token\-firma dei certificati a utenti malintenzionati di modificare o contraffare i token di sicurezza nel tentativo di accesso non autorizzato alle risorse federative. Ogni token\-certificato di firma contiene chiavi private di crittografia e le chiavi pubbliche utilizzate per firmare digitalmente \(tramite la chiave privata\) un token di sicurezza. In un secondo momento, dopo che queste chiavi vengono ricevute da un server federativo del partner, convalidano l'autenticità \(tramite la chiave pubblica\) del token di sicurezza crittografato.  
  
> [!CAUTION]  
> I certificati usati per il token\-firma sono fondamentali per la stabilità del servizio federativo. Poiché la perdita o non pianificato per la rimozione di tutti i certificati configurati per questo scopo può causare l'interruzione del servizio, è consigliabile eseguire backup tutti i certificati configurati per questo scopo.  
  
Il token\-certificato di firma dovrebbe essere concatenato a una radice attendibile nel servizio federativo. È possibile usare la procedura seguente per aggiungere il token\-certificato di firma per lo snap di gestione di AD FS\-in da un file che è stata esportata.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Per aggiungere un token\-certificato di firma  
  
1.  Nel **avviare** digitare**gestione di AD FS**, e quindi premere INVIO.  
  
2.  Nell'albero della console, fare doppio\-fare clic su **Service**, quindi fare clic su **certificati**.  
  
3.  Nel **azioni** riquadro, fare clic sul **Add Token\-Signing Certificate** collegamento.  
  
4.  Nel **cerca i file di certificato** finestra di dialogo passare al file del certificato che si desidera aggiungere, selezionare il file del certificato e quindi fare clic su **Open**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

