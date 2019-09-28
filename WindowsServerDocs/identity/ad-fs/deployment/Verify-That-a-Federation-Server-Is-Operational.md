---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: Verificare che un server federativo sia operativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6d27c8d69affe001630d8deaa2c21f334f8f86ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408314"
---
# <a name="verify-that-a-federation-server-is-operational"></a>Verificare che un server federativo sia operativo


Le procedure seguenti possono essere utilizzate per verificare che il server federativo è operativo, ovvero che qualsiasi client della stessa rete può raggiungere un nuovo server federativo.  
  
Per eseguire questa procedura è richiesta almeno l'appartenenza al gruppo **Users**, **Backup Operators**, **Power Users**, **Administrators** oppure a un gruppo equivalente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedura 1: Per verificare che un server federativo sia operativo  
  
1.  Per verificare che Internet Information Services \(IIS\) sia configurato correttamente nel server federativo, accedere a un computer client che si trova nella stessa foresta del server federativo.  
  
2.  Aprire una finestra del browser, nella barra degli indirizzi digitare il nome host DNS del server federativo, quindi aggiungere/ADFS/fs/FederationServerService.asmx per il nuovo server federativo, ad esempio:  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Premere INVIO, quindi completare la procedura successiva sul computer server federativo. Se viene visualizzato il messaggio si **è verificato un problema con il certificato di sicurezza del sito Web**, fare clic su continuare con il **sito Web**.  
  
    L'output previsto è una visualizzazione XML con il documento di descrizione del servizio. Se viene visualizzata questa pagina, IIS è operativo sul server federativo e serve le pagine.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedura 2: Per verificare che un server federativo sia operativo  
  
1.  Accedere al nuovo server federativo come amministratore.  
  
2.  Nella schermata **Start** Digitare **Visualizzatore eventi**e quindi premere INVIO.  
  
3.  Nel riquadro dei dettagli fare doppio\-clic su **registri applicazioni e servizi**,\-fare doppio clic su **ad FS evento**, quindi fare clic su **Amministrazione**.  
  
4.  Nella colonna **ID evento** cercare l'id evento 100. Se il server federativo è configurato correttamente, viene visualizzato un nuovo evento, nel registro applicazioni di Visualizzatore eventi, con l'ID evento 100. Questo evento verifica che il server federativo sia stato in grado di comunicare correttamente con il Servizio federativo.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

