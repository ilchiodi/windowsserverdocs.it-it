---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: Verificare che un server federativo sia operativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b9498451e8f6d7701e9ed4b3ac7d61f19d2dcdb4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191887"
---
# <a name="verify-that-a-federation-server-is-operational"></a>Verificare che un server federativo sia operativo


Le procedure seguenti possono essere utilizzate per verificare che il server federativo è operativo, ovvero che qualsiasi client della stessa rete può raggiungere un nuovo server federativo.  
  
Per eseguire questa procedura è richiesta almeno l'appartenenza al gruppo **Users**, **Backup Operators**, **Power Users**, **Administrators** oppure a un gruppo equivalente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedura 1: Per verificare che un server federativo sia operativo  
  
1.  Per verificare che Internet Information Services \(IIS\) sia configurato correttamente nel server federativo, accedere a un computer client che si trova nella stessa foresta del server federativo.  
  
2.  Aprire una finestra del browser, nella barra degli indirizzi digitare il federazione nome host DNS del server e quindi accodarvi /adfs/fs/federationserverservice.asmx per il nuovo server federativo, ad esempio:  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Premere INVIO, quindi completare la procedura successiva sul computer server federativo. Se viene visualizzato il messaggio **Si è verificato un problema con il certificato di protezione del sito Web**, fare clic su **Continuare con il sito Web**.  
  
    L'output previsto è una visualizzazione XML con il documento di descrizione del servizio. Se viene visualizzata questa pagina, IIS è operativo sul server federativo e serve le pagine.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedura 2: Per verificare che un server federativo sia operativo  
  
1.  Accedere al nuovo server federativo come amministratore.  
  
2.  Nel **avviare** digitare **Visualizzatore eventi**, quindi premere INVIO.  
  
3.  Nel riquadro dei dettagli fare doppio\-fare clic su **registri applicazioni e servizi**, double\-fare clic su **gestione eventi ADFS**, quindi fare clic su **Admin**.  
  
4.  Nel **ID evento** colonne, cercare l'evento ID 100. Se il server federativo è configurato correttamente, viene visualizzato un nuovo evento, ovvero nel registro applicazioni del Visualizzatore eventi, ovvero con l'evento ID 100. Tale evento verifica che il server federativo è stato in grado di comunicare correttamente con il servizio federativo.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

