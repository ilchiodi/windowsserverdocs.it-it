---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Impostare un certificato per le comunicazioni di servizi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7253502390db004747d3732cf3d288a51afdaaf1
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280694"
---
# <a name="set-a-service-communications-certificate"></a>Impostare un certificato per le comunicazioni di servizi


Server federativo in Active Directory Federation Services \(ADFS\) usare il certificato di comunicazioni di servizi per proteggere il traffico dei servizi Web per Secure Sockets Layer \(SSL\) la comunicazione con Web i client o server federativi.

> [!NOTE]  
> Il certificato per le comunicazioni del servizio non è quello utilizzato per un certificato SSL. Per modificare il certificato SSL di AD FS, è necessario usare Powershell. Seguire le indicazioni fornite in questo [articolo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


È possibile usare la procedura seguente per modificare il certificato di comunicazioni di servizi con lo snap di gestione di AD FS\-in.  

> [!NOTE]  
> Lo snap di gestione di AD FS\-in si riferisce ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.  

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-set-a-service-communications-certificate"></a>Per impostare un certificato di comunicazioni di servizi  

1.  Nel **avviare** digitare**gestione di AD FS**, e quindi premere INVIO.  

2.  Nell'albero della console, fare doppio\-fare clic su **Service**, quindi fare clic su **certificati**.  

3.  Nel **azioni** riquadro, fare clic sui **certificato per le comunicazioni del servizio impostato** collegamento.  

4.  Nel **selezionare un certificato di comunicazioni di servizi** finestra di dialogo passare al file del certificato che si desidera impostare come certificato per le comunicazioni del servizio, selezionare il file del certificato e quindi fare clic su **aprire**.  

## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
