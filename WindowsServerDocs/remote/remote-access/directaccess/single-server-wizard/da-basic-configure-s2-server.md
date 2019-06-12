---
title: Passaggio 2 configurare il Server DirectAccess di base
description: Questo argomento fa parte della Guida di distribuire un Server DirectAccess singolo con l'introduzione avvio procedura guidata per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82bf5fed-93b3-4fa6-8e71-522146eccdb1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9983fb475143109d191f3b6d69afef48d109472a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446956"
---
# <a name="step-2-configure-the-basic-directaccess-server"></a>Passaggio 2 configurare il Server DirectAccess di base

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive come configurare le impostazioni client e server richieste per una distribuzione di base di DirectAccess. Prima di iniziare la procedura di distribuzione, assicurarsi di aver completato i passaggi di pianificazione descritti in [pianificare una distribuzione DirectAccess base](Plan-a-Basic-DirectAccess-Deployment.md).  
  
|Attività|Descrizione|  
|----|--------|  
|Installare il ruolo Accesso remoto|Consente di installare il ruolo Accesso remoto.|  
|Configurare DirectAccess con Attività iniziali guidate|La nuova procedura Attività iniziali guidate semplifica notevolmente l'esperienza di configurazione. Questa procedura guidata maschera la complessità di DirectAccess e supporta una configurazione automatizzata in pochi semplici passaggi. La procedura guidata fornisce un'esperienza ottimale per l'amministratore, grazie alla configurazione automatica del proxy Kerberos che elimina la necessità di una distribuzione PKI interna.|  
|Aggiornare i client con la configurazione di DirectAccess|Per ricevere le impostazioni di DirectAccess, i client devono aggiornare Criteri di gruppo mentre sono connessi alla rete Intranet.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Installare il ruolo Accesso remoto  
Per distribuire Accesso remoto, è necessario installare il ruolo Accesso remoto in un server dell'organizzazione che fungerà da server di Accesso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Per installare il ruolo Accesso remoto  
  
1.  Nel server di accesso remoto, nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** per tre volte per visualizzare la schermata di selezione del ruolo server.  
  
3.  Nella finestra di dialogo **Selezione ruoli server** selezionare **Accesso remoto**e quindi fare clic su **Avanti**.  
  
4.  Nella finestra di dialogo **Selezione funzionalità** fare clic su **Avanti**.  
  
5.  Fare clic su **Avanti**, e quindi scegliere il **Selezione servizi ruolo** finestra di dialogo, fare clic sul **DirectAccess e VPN (RAS)** casella di controllo.  
  
6.  Fare clic su **Aggiungi funzionalità**, fare clic su **Avanti**, quindi fare clic su **installare**.  
  
7.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione sia stata completata correttamente e quindi fare clic su **Chiudi**.  
  
![Windows PowerShell](../../../media/Step-2-Configure-the-DirectAccess-Server/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il seguente cmdlet Windows PowerShell o i cmdlet installa il ruolo Accesso remoto: 

1. Aprire PowerShell come amministratore.

2. Installare la funzionalità di accesso remoto:

   ```  
   Install-WindowsFeature RemoteAccess   
   ```  

3. Riavviare il computer:

   ```
   Restart-Computer
   ```
   
4. Installare PowerShell di accesso remoto:

   ```
   Install-WindowsFeature RSAT-RemoteAccess-PowerShell
   ```



  
## <a name="configure-directaccess-with-the-getting-started-wizard"></a>Configurare DirectAccess con Attività iniziali guidate  
  
#### <a name="to-configure-directaccess-using-the-getting-started-wizard"></a>Per configurare DirectAccess con Attività iniziali guidate  
  
1.  In Server Manager fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Nella console di gestione accesso remoto, selezionare il servizio di ruolo da configurare nel riquadro di spostamento a sinistra e quindi fare clic su **eseguire la procedura guidata introduttiva**.  
  
3.  Fare clic su **distribuire DirectAccess solo**.  
  
4.  Selezionare la topologia della configurazione di rete e digitare il nome pubblico a cui si connetteranno i client di accesso remoto. Fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Per impostazione predefinita, la procedura Attività iniziali guidate consente di distribuire DirectAccess in tutti i computer portatili e notebook nel dominio applicando un filtro WMI all'oggetto Criteri di gruppo delle impostazioni client.  
  
5.  Scegliere **Fine**.  
  
6.  Poiché in questa distribuzione non viene usata alcuna infrastruttura a chiave pubblica (PKI), se i certificati non vengono trovati la procedura guidata effettua automaticamente il provisioning di certificati autofirmati per IP-HTTPS e il server dei percorsi di rete e abilita automaticamente il proxy Kerberos. oltre a NAT64 e DNS64 per la traduzione dei protocolli nell'ambiente solo IPv4. Dopo che la procedura guidata ha applicato la configurazione, fare clic su **Chiudi**.  
  
7.  Nell'albero della Console di gestione Accesso remoto selezionare **Stato operazioni**. Attendere che lo stato di tutte le operazioni di monitoraggio venga visualizzato come "Funzionante". Nel riquadro Attività sotto Monitoraggio fare clic su **Aggiorna** periodicamente per aggiornare la visualizzazione.  
  
## <a name="update-clients-with-the-directaccess-configuration"></a>Aggiornare i client con la configurazione di DirectAccess  
  
#### <a name="to-update-directaccess-clients"></a>Per aggiornare i client DirectAccess  
  
1.  Aprire PowerShell come amministratore.  
  
2.  Nella finestra di PowerShell digitare **gpupdate** e quindi premere **INVIO**.  
  
3.  Attendere il completamento dell'aggiornamento dei criteri computer.  
  
4.  Digitare **Get-DnsClientNrptPolicy** e quindi premere **INVIO**  
  
    Vengono visualizzate le voci della tabella dei criteri di risoluzione dei nomi per DirectAccess. Si noti che è visualizzata l'esenzione del server NLS. La procedura Attività iniziali guidate ha creato automaticamente questa voce DNS per il server DirectAccess e ha effettuato il provisioning di un certificato autofirmato associato, in modo che il server DirectAccess possa funzionare come server dei percorsi di rete.  
  
5.  Digitare **Get-NCSIPolicyConfiguration** e quindi premere **INVIO**. Vengono visualizzate le impostazioni dell'indicatore di stato della connettività di rete distribuite dalla procedura guidata. Si noti il valore di DomainLocationDeterminationURL. Ogni volta che questo URL del server dei percorsi di rete è accessibile, il client determinerà che si trova all'interno della rete aziendale e le impostazioni della tabella dei criteri di risoluzione dei nomi non verranno applicate.  
  
6.  Tipo **Get-DAConnectionStatus** e quindi premere **INVIO**. Poiché il client possa raggiungere l'URL di server di percorso di rete, lo stato verrà visualizzato come **ConnectedLocally**.  
  
## <a name="BKMK_Links"></a>Passaggio precedente  
  
-   [Passaggio 1: Configurare l'infrastruttura di DirectAccess](Step-1-Configure-the-DirectAccess-Infrastructure.md)  
  
## <a name="next-step"></a>Passaggio successivo  
  
-   [Passaggio 3 verificare le distribuzioni di DirectAccess di base](da-basic-configure-s3-verify.md)  
  


