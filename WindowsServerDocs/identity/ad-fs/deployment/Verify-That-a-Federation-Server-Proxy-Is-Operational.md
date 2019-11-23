---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Verificare il funzionamento di un proxy server federativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 24f4fe2a152244dc904be82c4c10abe71abffcc4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359974"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Verificare il funzionamento di un proxy server federativo


È possibile utilizzare la procedura seguente per verificare che il proxy server federativo sia in grado di comunicare con il Servizio federativo in Active Directory Federation Services \(AD FS\). Questa procedura viene eseguita dopo l'esecuzione della **Configurazione guidata del proxy server federativo di ad FS** per configurare il computer per l'esecuzione nel ruolo proxy server federativo. Per ulteriori informazioni su come eseguire questa procedura guidata, vedere [configurare un computer per il ruolo proxy server federativo](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> Il risultato di questa verifica è la generazione di un evento specifico nel Visualizzatore eventi del computer proxy server federativo.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Per verificare che un proxy server federativo sia operativo  
  
1.  Accedere al proxy server federativo come amministratore.  
  
2.  Nella schermata **Start** Digitare**Visualizzatore eventi**e quindi premere INVIO.  
  
3.  Nel riquadro dei dettagli, fare doppio\-fare clic su **registri applicazioni e servizi**, fare doppio\-fare clic su **ad FS evento**, quindi fare clic su **Amministrazione**.  
  
4.  Nella colonna **ID evento**, cercare l'evento ID 198.  
  
    Se il proxy server federativo è configurato correttamente, viene visualizzato un nuovo evento nel registro applicazioni di Visualizzatore eventi con l'ID evento 198. Questo evento verifica che il servizio proxy server federativo sia stato avviato correttamente e che ora sia online.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

