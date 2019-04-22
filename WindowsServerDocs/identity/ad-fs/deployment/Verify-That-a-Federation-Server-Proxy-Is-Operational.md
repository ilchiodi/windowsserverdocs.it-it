---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Verificare il funzionamento di un proxy server federativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814622"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Verificare il funzionamento di un proxy server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile usare la procedura seguente per verificare che il proxy server federativo possa comunicare con il servizio federativo in Active Directory Federation Services \(ADFS\). Eseguire questa procedura dopo aver eseguito il **configurazione guidata di AD FS Federation Server Proxy** per configurare il computer per l'esecuzione nel ruolo proxy server federativo. Per altre informazioni su come eseguire questa procedura guidata, vedere [configurare un Computer per il ruolo Proxy Server federativo](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> Il risultato di questa verifica è la generazione di un evento specifico nel Visualizzatore eventi del computer proxy server federativo.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Per verificare che un proxy server federativo sia operativo  
  
1.  Accedere al proxy server federativo come amministratore.  
  
2.  Nel **avviare** digitare**Visualizzatore eventi**, quindi premere INVIO.  
  
3.  Nel riquadro dei dettagli fare doppio\-fare clic su **registri applicazioni e servizi**, double\-fare clic su **gestione eventi ADFS**, quindi fare clic su **Admin**.  
  
4.  Nella colonna **ID evento**, cercare l'evento ID 198.  
  
    Se il proxy server federativo è configurato correttamente, viene visualizzato un nuovo evento nel registro applicazioni del Visualizzatore eventi, con l'evento ID 198. Tale evento verifica che il servizio di proxy server federativo è stato avviato e adesso è online.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

