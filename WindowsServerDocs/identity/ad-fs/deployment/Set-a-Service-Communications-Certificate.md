---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Impostare un certificato per le comunicazioni di servizi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d0464853c73f88ed76545921ffc8a4bf8551c800
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408329"
---
# <a name="set-a-service-communications-certificate"></a>Impostare un certificato per le comunicazioni di servizi


Server federativi in Active Directory Federation Services \(AD FS @ no__t-1 usare il certificato di comunicazione del servizio per proteggere il traffico dei servizi Web per la comunicazione Secure Sockets Layer \(SSL @ no__t-3 con client Web o con server federativo proxy.

> [!NOTE]  
> Il certificato di comunicazione del servizio non corrisponde a un certificato SSL. Per modificare il certificato SSL di AD FS, sarà necessario usare PowerShell. Seguire le istruzioni riportate in questo [articolo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


È possibile utilizzare la procedura seguente per modificare il certificato di comunicazione del servizio con lo snap-in di gestione di AD FS @ no__t-0cm.  

> [!NOTE]  
> Lo snap-in di gestione AD FS @ no__t-0cm fa riferimento ai certificati di autenticazione server per i server federativi come certificati di comunicazione del servizio.  

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere \( [gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) di\/dominio http:\/\/go.Microsoft.com\/fwlink? LinkId\=83477\).   

### <a name="to-set-a-service-communications-certificate"></a>Per impostare un certificato per le comunicazioni di servizio  

1.  Nella schermata **Start** Digitare**ad FS Management**, quindi premere INVIO.  

2.  Nell'albero della console, Double @ no__t-0click **Service**, quindi fare clic su **Certificates**.  

3.  Nel riquadro **azioni** fare clic sul collegamento **set service Communications Certificate** .  

4.  Nella finestra di dialogo **selezionare un certificato di comunicazione del servizio** passare al file del certificato che si desidera impostare come certificato di comunicazione del servizio, selezionare il file del certificato e quindi fare clic su **Apri**.  

## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
