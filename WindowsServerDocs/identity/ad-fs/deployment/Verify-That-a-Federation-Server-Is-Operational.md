---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: Verificare che un Server federativo sia operativo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2034b4c35061879a64004486395d0887c59087b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-is-operational"></a>Verificare che un Server federativo sia operativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare le procedure seguenti per verificare che un server federativo sia operativo; vale a dire che qualsiasi client nella stessa rete può raggiungere un nuovo server federativo.  
  
Appartenenza al gruppo **utenti**, **Backup Operators**, **Power Users**, **amministratori** o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedura 1: Verificare che un server federativo sia operativo  
  
1.  Per verificare che Internet Information Services \(IIS\) sia configurato correttamente nel server federativo, accedere a un computer client che si trova nella stessa foresta del server federativo.  
  
2.  Aprire una finestra del browser, del tipo di barra indirizzo federazione nome host DNS del server e quindi aggiungervi /adfs/fs/federationserverservice.asmx per il nuovo server federativo, ad esempio:  
  
    **https://FS1.fabrikam.com/adfs/fs/FederationServerService.asmx**  
  
3.  Premere INVIO e quindi completare la procedura successiva sul computer del server federativo. Se viene visualizzato il messaggio **si è verificato un problema con il certificato di sicurezza del sito Web**, fare clic su **continua con il sito Web**.  
  
    L'output previsto è una visualizzazione XML con il documento di descrizione del servizio. Se viene visualizzata questa pagina, IIS nel server federativo è operative e serve le pagine correttamente.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedura 2: Verificare che un server federativo sia operativo  
  
1.  Accedere al nuovo server federativo come amministratore.  
  
2.  Nel **Start** digitare **Visualizzatore eventi**, quindi premere INVIO.  
  
3.  Nel riquadro dei dettagli, fare doppio clic **registri applicazioni e servizi**, fare doppio clic **AD FS Eventing**, quindi fare clic su **Admin**.  
  
4.  Nel **ID evento** colonna, cercare l'evento ID 100. Se il server federativo è configurato correttamente, visualizzato un nuovo evento, nel registro delle applicazioni del Visualizzatore eventi, con l'evento ID 100. Questo evento verifica che il server federativo è stato in grado di comunicare con il servizio federativo.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

