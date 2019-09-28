---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importare un certificato di autenticazione server nel sito Web predefinito
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b1c602d0cdfa562469419de223f5691ec2ff4527
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359558"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importare un certificato di autenticazione server nel sito Web predefinito

Dopo aver ottenuto un certificato di autenticazione server da un'autorità di certificazione \(CA @ no__t-1, è necessario installare manualmente il certificato nel sito Web predefinito per ogni server federativo o proxy server federativo in una server farm.  
  
Per i server Web, è necessario installare manualmente il certificato di autenticazione server nel sito Web appropriato o nella directory virtuale in cui risiede l'applicazione federata.  
  
Se si configura una farm, assicurarsi di eseguire questa procedura in modo identico, usando le stesse identiche impostazioni, in ogni server della farm.  
  
> [!NOTE]  
> Lo snap-in di gestione AD FS @ no__t-0cm fa riferimento ai certificati di autenticazione server per i server federativi come certificati di comunicazione del servizio.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Per importare un certificato di autenticazione server nel sito Web predefinito  
  
1.  Nella schermata **Start** digitare**Internet Information Services \(IIS @ No__t-3 Manager**, quindi premere INVIO.  
  
2.  Nell'albero della console fare clic su **Nome computer**.  
  
3.  Nel riquadro centrale, Double @ no__t-0click **Server Certificates**.  
  
4.  Nel riquadro **Azioni** fare clic su **Importa**.  
  
5.  Nella finestra di dialogo **Importa certificato** fare clic su **...** .  
  
6.  Passare al percorso del file di certificato pfx, evidenziarlo e quindi fare clic su **Apri**.  
  
7.  Immettere una password per il certificato e quindi fare clic su **OK**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisiti dei certificati per i proxy server federativi](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

