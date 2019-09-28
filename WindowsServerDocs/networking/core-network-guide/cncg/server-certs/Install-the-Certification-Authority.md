---
title: Installare l'Autorità di certificazione
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 024fc73c4ed089d81808cf44d7cfe8b01bfffaa0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406322"
---
# <a name="install-the-certification-authority"></a>Installare l'Autorità di certificazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per installare Servizi certificati Active Directory (AD CS) in modo che è possibile registrare un certificato server da server che eseguono Server dei criteri di rete (NPS), Routing e accesso remoto (RRAS) o entrambi.  
  
> [!IMPORTANT]  
> -   Prima di installare Servizi certificati Active Directory, è necessario assegnare il nome del computer, configurare il computer con un indirizzo IP statico e aggiungere il computer al dominio. Per ulteriori informazioni su come eseguire queste attività, vedere Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).  
> -   Per eseguire questa procedura, il computer in cui si installa Servizi certificati Active Directory deve essere aggiunti a un dominio in cui è installato Servizi di dominio Active Directory (AD DS).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Enterprise Admins** e al gruppo **Domain Admins** del dominio radice.  
  
> [!NOTE]  
> Per eseguire questa procedura mediante Windows PowerShell, aprire Windows PowerShell e digitare il comando seguente e quindi premere INVIO.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Dopo aver installato Servizi certificati Active Directory, digitare il comando seguente e premere INVIO.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Per installare Servizi certificati Active Directory  

> [!TIP]
> Se si vuole usare Windows PowerShell per installare Active Directory Servizi certificati, vedere [Install-AdcsCertificationAuthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) per i cmdlet e i parametri facoltativi.
  
1.  Accedere come membro del gruppo Enterprise Admins e del gruppo Domain Admins del dominio radice.  
  
2.  In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.  
  
3.  In **Prima di iniziare** fare clic su **Avanti**.  
  
    > [!NOTE]  
    > La pagina **Prima di iniziare** dell'Aggiunta guidata ruoli e funzionalità non viene visualizzata se durante un'esecuzione precedente della procedura guidata è stata selezionata la casella di controllo **Ignora questa pagina per impostazione predefinita**.  
  
4.  In **Select Installation Type**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.  
  
5.  In **Selezione server di destinazione** assicurarsi che sia selezionata l'opzione **Selezionare un server dal pool di server**. Assicurarsi che in **Pool di server** sia selezionato il computer locale. Fare clic su **Avanti**.  
  
6.  In **Selezione ruoli Server**, in **ruoli**, selezionare **Servizi certificati Active Directory**. Quando viene chiesto di aggiungere le funzionalità necessarie, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
7.  In **Selezionare le funzionalità**, fare clic su **Avanti**.  
  
8.  In **Servizi certificati Active Directory**, leggere le informazioni fornite e quindi fare clic su **Avanti**.  
  
9. In **Conferma selezioni per l'installazione**, fare clic su **installare**. Non chiudere la procedura guidata durante il processo di installazione. Quando l'installazione è stata completata, fare clic su **configurare Active Directory i servizi certificati nel server di destinazione**. Verrà visualizzata la procedura guidata configurazione AD CS. Leggere le informazioni sulle credenziali e, se necessario, fornire le credenziali per un account membro del gruppo Enterprise Admins. Fare clic su **Avanti**.  
  
10. In **servizi ruolo**, fare clic su **autorità di certificazione**, quindi fare clic su **Avanti**.  
  
11. Nel **tipo di installazione** verificare che **CA dell'organizzazione** sia selezionata e quindi fare clic su **Avanti**.  
  
12. Nel **specificare il tipo di CA** verificare che **CA radice** sia selezionata e quindi fare clic su **Avanti**.  
  
13. Nel **specificare il tipo della chiave privata** verificare che **creare una nuova chiave privata** sia selezionata e quindi fare clic su **Avanti**.  
  
14. Nella pagina **crittografia per la CA** , conservare le impostazioni predefinite per CSP (**RSA # Microsoft software key storage provider**) e l'algoritmo hash (**SHA2**) e determinare la lunghezza dei caratteri chiave migliore per la distribuzione. Lunghezze di chiave in caratteri grandi offrono una sicurezza ottimale; Tuttavia, che può influire sulle prestazioni del server e potrebbe non essere compatibile con applicazioni legacy. È consigliabile mantenere l'impostazione predefinita di 2048. Fare clic su **Avanti**.  
  
15. Nel **nome CA** pagina, mantenere il nome comune suggerito per l'autorità di certificazione o modificare il nome in base ai requisiti. Verificare che si è certi che il nome della CA è compatibile con le convenzioni di denominazione e scopi, poiché non è possibile modificare il nome della CA dopo aver installato Servizi certificati Active Directory. Fare clic su **Avanti**.  
  
16. Nel **periodo di validità** pagina **specificare il periodo di validità**, digitare il numero e selezionare un valore di tempo (anni, mesi, settimane o giorni). È consigliabile utilizzare l'impostazione predefinita di cinque anni. Fare clic su **Avanti**.  
  
17. Nel **Database CA** pagina **specificare i percorsi dei database**, specificare il percorso della cartella per il database dei certificati e registro database certificati. Se si specificano percorsi diversi da quelli predefiniti, verificare che le cartelle siano protette tramite elenchi di controllo di accesso (ACL) che impediscano a utenti e computer non autorizzati l'accesso al database della CA e ai file di registro. Fare clic su **Avanti**.  
  
18. In **conferma**, fare clic su **Configura** per applicare le selezioni, quindi fare clic su **Chiudi**.  
  


