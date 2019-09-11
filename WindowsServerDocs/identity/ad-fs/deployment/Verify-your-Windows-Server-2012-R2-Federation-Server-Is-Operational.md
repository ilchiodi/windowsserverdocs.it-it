---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: Verificare che il server federativo di Windows Server 2012 R2 sia operativo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d32338d0e9242d12ab18fd30192736ea3b44fdb4
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868060"
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>Verificare che il server federativo di Windows Server 2012 R2 sia operativo



Le procedure seguenti possono essere utilizzate per verificare che il server federativo è operativo, ovvero che qualsiasi client della stessa rete può raggiungere un nuovo server federativo.  
  
Per eseguire questa procedura è richiesta almeno l'appartenenza al gruppo **Users**, **Backup Operators**, **Power Users**, **Administrators** oppure a un gruppo equivalente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedura 1: Per verificare che un server federativo sia operativo  
  
1.  Per verificare che Internet Information Services \(IIS\) sia configurato correttamente nel server federativo, accedere a un computer client che si trova nella stessa foresta del server federativo.  
  
2.  Aprire una finestra del browser, nella barra degli indirizzi digitare il nome host DNS del server federativo, quindi \/accodare\/ADFS\/fs FederationServerService. asmx per il nuovo server federativo, ad esempio:  
  
    **https:\/\/FS1.fabrikam.comADFS\/fsFederationServerService\/. asmx\/**  
  
3.  Premere INVIO, quindi completare la procedura successiva sul computer server federativo. Se viene visualizzato il messaggio si **è verificato un problema con il certificato di sicurezza del sito Web**, fare clic su continuare con il **sito Web**.  
  
    L'output previsto è una visualizzazione XML con il documento di descrizione del servizio. Se viene visualizzata questa pagina, IIS è operativo sul server federativo e serve le pagine.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedura 2: Per verificare che un server federativo sia operativo  
  
1.  Accedere al nuovo server federativo come amministratore.  
  
2.  Nella schermata **Start** Digitare**Visualizzatore eventi**e quindi premere INVIO.  
  
3.  Nel riquadro dei dettagli fare doppio\-clic su **registri applicazioni e servizi**,\-fare doppio clic su **ad FS evento**, quindi fare clic su **Amministrazione**.  
  
4.  Nella colonna **ID evento** cercare l'id evento 100. Se il server federativo è configurato correttamente, viene visualizzato un nuovo evento, nel registro applicazioni di Visualizzatore eventi, con l'ID evento 100. Questo evento verifica che il server federativo sia stato in grado di comunicare correttamente con il Servizio federativo.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
   
  

