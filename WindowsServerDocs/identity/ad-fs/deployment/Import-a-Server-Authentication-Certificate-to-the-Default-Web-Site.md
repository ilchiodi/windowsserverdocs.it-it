---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importare un certificato di autenticazione server nel sito Web predefinito
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3f02358167b024247f934a46218028575e393ba9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855404"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importare un certificato di autenticazione server nel sito Web predefinito

Dopo aver ottenuto un certificato di autenticazione server da un'autorità di certificazione \(CA\), è necessario installare manualmente il certificato nel sito Web predefinito per ogni server federativo o proxy server federativo in un server farm.  
  
Per i server Web, è necessario installare manualmente il certificato di autenticazione server nel sito Web appropriato o nella directory virtuale in cui risiede l'applicazione federata.  
  
Se si configura una farm, assicurarsi di eseguire questa procedura in modo identico, usando le stesse identiche impostazioni, in ogni server della farm.  
  
> [!NOTE]  
> Lo snap\-di gestione AD FS in fa riferimento ai certificati di autenticazione server per i server federativi come certificati di comunicazione del servizio.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Importazione di un certificato di autenticazione server nel sito Web predefinito  
  
1.  Nella schermata **Start** digitare**Internet Information Services \(IIS\) Manager**, quindi premere INVIO.  
  
2.  Nell'albero della console fare clic su **NomeComputer**.  
  
3.  Nel riquadro centrale, fare doppio\-fare clic su **certificati server**.  
  
4.  Nel riquadro **Azioni** fare clic su **Importa**.  
  
5.  Nella finestra di dialogo **Importa certificato** fare clic su **...** .  
  
6.  Passare al percorso del file di certificato con estensione pfx, evidenziarlo e quindi fare clic su **Apri**.  
  
7.  Digitare una password per il certificato, quindi fare clic su **OK**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisiti dei certificati per i proxy server federativi](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

