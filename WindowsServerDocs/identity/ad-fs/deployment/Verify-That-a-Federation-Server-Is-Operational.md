---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: Verificare che un server federativo sia operativo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b6ce4f826809e2c07fdbe4424d427f1244ea5172
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814254"
---
# <a name="verify-that-a-federation-server-is-operational"></a>Verificare che un server federativo sia operativo


Le procedure seguenti possono essere utilizzate per verificare che il server federativo è operativo, ovvero che qualsiasi client della stessa rete può raggiungere un nuovo server federativo.  
  
Come requisito minimo per eseguire questa procedura è richiesta l'appartenenza al gruppo **Utenti**, **Backup Operators**, **Power Users** o **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedura 1: per verificare che un server federativo sia operativo  
  
1.  Per verificare che Internet Information Services \(IIS\) sia configurato correttamente nel server federativo, accedere a un computer client che si trova nella stessa foresta del server federativo.  
  
2.  Aprire una finestra del browser, nella barra degli indirizzi digitare il nome host DNS del server federativo, quindi aggiungere/ADFS/fs/FederationServerService.asmx per il nuovo server federativo, ad esempio:  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Premere INVIO, quindi completare la procedura successiva sul computer server federativo. Se viene visualizzato il messaggio si **è verificato un problema con il certificato di sicurezza del sito Web**, fare clic su continuare con il **sito Web**.  
  
    L'output previsto è una visualizzazione XML con il documento di descrizione del servizio. Se viene visualizzata questa pagina, IIS è operativo sul server federativo e serve le pagine.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedura 2: per verificare che un server federativo sia operativo  
  
1.  Accedere al nuovo server federativo come amministratori.  
  
2.  Nella schermata **Start** digitare **Visualizzatore eventi** e premere INVIO.  
  
3.  Nel riquadro dei dettagli, fare doppio\-fare clic su **registri applicazioni e servizi**, fare doppio\-fare clic su **ad FS evento**, quindi fare clic su **Amministrazione**.  
  
4.  Nella colonna **ID evento** cercare l'id evento 100. Se il server federativo è configurato correttamente, viene visualizzato un nuovo evento, nel registro applicazioni di Visualizzatore eventi, con l'ID evento 100. Questo evento verifica che il server federativo sia stato in grado di comunicare correttamente con il Servizio federativo.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

