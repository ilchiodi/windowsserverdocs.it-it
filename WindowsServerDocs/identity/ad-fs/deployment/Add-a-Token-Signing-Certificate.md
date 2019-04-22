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
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826152"
---
# <a name="add-a-token-signing-certificate"></a>Aggiungere un certificato per la firma di token

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

