---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Impostare un certificato per le comunicazioni di servizi
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6e0f9e6cca4fe915d3faed77fd5b5db543596d70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855304"
---
# <a name="set-a-service-communications-certificate"></a>Impostare un certificato per le comunicazioni di servizi


Server federativi in Active Directory Federation Services \(AD FS\) utilizzare il certificato di comunicazione del servizio per proteggere il traffico dei servizi Web per Secure Sockets Layer \(la comunicazione SSL\) con client Web o con proxy server federativi.

> [!NOTE]  
> Il certificato di comunicazione del servizio non corrisponde a un certificato SSL. Per modificare il certificato SSL di AD FS, sarà necessario usare PowerShell. Seguire le istruzioni riportate in questo [articolo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


È possibile utilizzare la procedura seguente per modificare il certificato di comunicazione del servizio con lo snap-in di gestione AD FS\-in.  

> [!NOTE]  
> Lo snap\-di gestione AD FS in fa riferimento ai certificati di autenticazione server per i server federativi come certificati di comunicazione del servizio.  

L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-set-a-service-communications-certificate"></a>Per impostare un certificato per le comunicazioni di servizio  

1.  Nella schermata **Start** Digitare**ad FS Management**, quindi premere INVIO.  

2.  Nell'albero della console fare doppio\-fare clic su **servizio**, quindi fare clic su **certificati**.  

3.  Nel riquadro **azioni** fare clic sul collegamento **set service Communications Certificate** .  

4.  Nella finestra di dialogo **selezionare un certificato di comunicazione del servizio** passare al file del certificato che si desidera impostare come certificato di comunicazione del servizio, selezionare il file del certificato e quindi fare clic su **Apri**.  

## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
