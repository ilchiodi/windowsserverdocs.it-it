---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importare un certificato di autenticazione Server per il sito Web predefinito
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importare un certificato di autenticazione Server per il sito Web predefinito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver ottenuto un server di certificato di autenticazione da un'autorità di certificazione \(CA\), è necessario installare manualmente quel certificato nel sito Web predefinito per ogni server federativo o proxy server federativo in una server farm.  
  
Per i server Web, è necessario installare manualmente il certificato di autenticazione server nel sito Web appropriato o la directory virtuale in cui risiede l'applicazione federata.  
  
Se si configura una farm, assicurarsi di eseguire questa procedura in modo identico, usando le stesse identiche impostazioni, in ogni server della farm.  
  
> [!NOTE]  
> Snap-in di gestione di ADFS fa riferimento ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Per importare un certificato di autenticazione server nel sito Web predefinito  
  
1.  Nel **Start** digitare**\(IIS\) Internet Information Services Manager**, quindi premere INVIO.  
  
2.  Nell'albero della console, fare clic su **ComputerName**.  
  
3.  Nel riquadro centrale fare doppio clic **i certificati Server**.  
  
4.  Nel **azioni** riquadro, fare clic su **importazione**.  
  
5.  Nel **Importa certificato** la finestra di dialogo, fare clic su di **...** pulsante.  
  
6.  Selezionare il percorso del file di certificato pfx, evidenziarlo e quindi fare clic su **aprire**.  
  
7.  Digitare una password per il certificato, quindi fare clic su **OK**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisiti dei certificati per i proxy Server federativi](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

