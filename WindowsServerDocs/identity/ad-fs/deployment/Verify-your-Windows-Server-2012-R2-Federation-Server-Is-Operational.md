---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: Verificare che il Server federativo di Windows Server 2012 R2 sia operativo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7cab415cc599f388c2bb5966d45998874ce56987
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191836"
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>Verificare che il Server federativo di Windows Server 2012 R2 sia operativo



Le procedure seguenti possono essere utilizzate per verificare che il server federativo è operativo, ovvero che qualsiasi client della stessa rete può raggiungere un nuovo server federativo.  
  
Per eseguire questa procedura è richiesta almeno l'appartenenza al gruppo **Users**, **Backup Operators**, **Power Users**, **Administrators** oppure a un gruppo equivalente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedura 1: Per verificare che un server federativo sia operativo  
  
1.  Per verificare che Internet Information Services \(IIS\) sia configurato correttamente nel server federativo, accedere a un computer client che si trova nella stessa foresta del server federativo.  
  
2.  Aprire una finestra del browser, nella barra degli indirizzi digitare nome host DNS del server federativo e quindi accodare \/adfs\/fs\/federationserverservice.asmx a esso per il nuovo server federativo, ad esempio:  
  
    **https:\/\/fs1.fabrikam.com\/adfs\/fs\/federationserverservice.asmx**  
  
3.  Premere INVIO, quindi completare la procedura successiva sul computer server federativo. Se viene visualizzato il messaggio **Si è verificato un problema con il certificato di protezione del sito Web**, fare clic su **Continuare con il sito Web**.  
  
    L'output previsto è una visualizzazione XML con il documento di descrizione del servizio. Se viene visualizzata questa pagina, IIS è operativo sul server federativo e serve le pagine.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedura 2: Per verificare che un server federativo sia operativo  
  
1.  Accedere al nuovo server federativo come amministratore.  
  
2.  Nel **avviare** digitare**Visualizzatore eventi**, quindi premere INVIO.  
  
3.  Nel riquadro dei dettagli fare doppio\-fare clic su **registri applicazioni e servizi**, double\-fare clic su **gestione eventi ADFS**, quindi fare clic su **Admin**.  
  
4.  Nel **ID evento** colonne, cercare l'evento ID 100. Se il server federativo è configurato correttamente, viene visualizzato un nuovo evento, ovvero nel registro applicazioni del Visualizzatore eventi, ovvero con l'evento ID 100. Tale evento verifica che il server federativo è stato in grado di comunicare correttamente con il servizio federativo.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
   
  

