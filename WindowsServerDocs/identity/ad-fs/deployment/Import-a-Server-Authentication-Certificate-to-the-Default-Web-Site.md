---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importare un certificato di autenticazione server nel sito Web predefinito
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880942"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importare un certificato di autenticazione server nel sito Web predefinito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver ottenuto un server di certificato di autenticazione da un'autorità di certificazione \(CA\), è necessario installare manualmente quel certificato nel sito Web predefinito per ogni server federativo o proxy server federativo in una server farm.  
  
Per i server Web, è necessario installare manualmente il certificato di autenticazione server nel sito Web appropriato o nella directory virtuale in cui risiede l'applicazione federata.  
  
Se si configura una farm, assicurarsi di eseguire questa procedura in modo identico, usando le stesse identiche impostazioni, in ogni server della farm.  
  
> [!NOTE]  
> Lo snap di gestione di AD FS\-in si riferisce ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Per importare un certificato di autenticazione server nel sito Web predefinito  
  
1.  Nel **avviare** digitare**Internet Information Services \(IIS\) Manager**, quindi premere INVIO.  
  
2.  Nell'albero della console fare clic su **Nome computer**.  
  
3.  Nel riquadro centrale fare doppio\-fare clic su **certificati Server**.  
  
4.  Nel riquadro **Azioni** fare clic su **Importa**.  
  
5.  Nel **importare il certificato** della finestra di dialogo fare clic su di **...** .  
  
6.  Passare al percorso del file di certificato pfx, evidenziarlo e quindi fare clic su **Apri**.  
  
7.  Immettere una password per il certificato e quindi fare clic su **OK**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisiti dei certificati per i Server federativi](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

