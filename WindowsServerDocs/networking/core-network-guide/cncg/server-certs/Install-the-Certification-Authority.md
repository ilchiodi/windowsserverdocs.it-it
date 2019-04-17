---
title: Installare l'autorità di certificazione
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17a887ab32570b739d5ca99611ee0d496a966d1b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-certification-authority"></a>Installare l'autorità di certificazione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per installare Servizi certificati Active Directory (AD CS) in modo che è possibile registrare un certificato server per server che eseguono Server dei criteri di rete (NPS), Routing e accesso remoto (RRAS) o entrambi.  
  
> [!IMPORTANT]  
> -   Prima di installare Servizi certificati Active Directory, è necessario il nome computer, configurare il computer con un indirizzo IP statico e aggiungere il computer al dominio. Per ulteriori informazioni su come eseguire queste attività, vedere Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).  
> -   Per eseguire questa procedura, il computer in cui si installa Servizi certificati Active Directory deve essere aggiunti a un dominio in cui è installato Servizi di dominio Active Directory (AD DS).  
  
L'appartenenza a entrambi **Enterprise Admins** e del dominio radice **Domain Admins** gruppo è il requisito minimo necessario per completare questa procedura.  
  
> [!NOTE]  
> Per eseguire questa procedura mediante Windows PowerShell, aprire Windows PowerShell e digitare il comando seguente e quindi premere INVIO.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Dopo aver installato Servizi certificati Active Directory, digitare il comando seguente e premere INVIO.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Per installare Servizi certificati Active Directory  
  
1.  Accedere come membro del gruppo Domain Admins del dominio radice e al gruppo Enterprise Admins.  
  
2.  In Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Verrà avviata l'aggiunta guidata ruoli e funzionalità.  
  
3.  In **prima di iniziare**, fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzato se è stato selezionato in precedenza **Ignora questa pagina per impostazione predefinita** quando è stato eseguito l'aggiunta guidata ruoli e funzionalità.  
  
4.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.  
  
5.  In **server di destinazione**, assicurarsi che **selezionare un server dal pool di server** sia selezionata. In **Pool di Server**, verificare che sia selezionato il computer locale. Fare clic su **Avanti**.  
  
6.  In **Selezione ruoli Server**, in **ruoli**selezionare **Servizi certificati Active Directory**. Quando viene chiesto di aggiungere le funzionalità necessarie, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
7.  In **selezionare le funzionalità**, fare clic su **Avanti**.  
  
8.  In **Servizi certificati Active Directory**, leggere le informazioni fornite e quindi fare clic su **Avanti**.  
  
9. In **Conferma selezioni per l'installazione**, fare clic su **installare**. Non chiudere la procedura guidata durante il processo di installazione. Quando l'installazione è stata completata, fare clic su **configurare dei servizi di certificati Active Directory nel server di destinazione**. Apre la procedura guidata configurazione AD CS. Leggere le informazioni sulle credenziali e, se necessario, fornire le credenziali per un account che sia membro del gruppo Enterprise Admins. Fare clic su **Avanti**.  
  
10. In **servizi ruolo**, fare clic su **autorità di certificazione**, quindi fare clic su **Avanti**.  
  
11. Nel **tipo di installazione** verificare che **CA dell'organizzazione** sia selezionata e quindi fare clic su **Avanti**.  
  
12. Nel **specificare il tipo di CA** verificare che **autorità di certificazione radice** sia selezionata e quindi fare clic su **Avanti**.  
  
13. Nel **specificare il tipo di chiave privata** verificare che **creare una nuova chiave privata** sia selezionata e quindi fare clic su **Avanti**.  
  
14. Nel **crittografia per la CA** pagina, mantenere le impostazioni predefinite per CSP (**RSA #Microsoft Provider di archiviazione chiave Software**) e l'algoritmo hash (**SHA1**) e determinare la lunghezza chiave in caratteri più appropriata per la distribuzione. Lunghezze di chiave in caratteri elevata offrono una sicurezza ottimale; Tuttavia, che possono influire sulle prestazioni di server e potrebbe non essere compatibile con applicazioni legacy. È consigliabile mantenere l'impostazione predefinita di 2048. Fare clic su **Avanti**.  
  
15. Nel **nome CA** pagina, mantenere il nome comune suggerito per l'autorità di certificazione o modificare il nome in base alle esigenze. Assicurarsi che si è certi che il nome della CA è compatibile con le convenzioni di denominazione e scopi, poiché non è possibile modificare il nome della CA dopo aver installato Servizi certificati Active Directory. Fare clic su **Avanti**.  
  
16. Nel **periodo di validità** pagina **specificare il periodo di validità**, digitare il numero e selezionare un valore di tempo (anni, mesi, settimane o giorni). È consigliabile l'impostazione predefinita di cinque anni. Fare clic su **Avanti**.  
  
17. Nel **Database CA** pagina **specificare i percorsi del database**, specificare il percorso della cartella per il database certificati e registro database certificati. Se si specificano percorsi diversi da quelli predefiniti, assicurarsi che le cartelle siano protette tramite elenchi di controllo di accesso (ACL) che impediscono l'accesso ai file di registro e il database CA di computer o utenti non autorizzati. Fare clic su **Avanti**.  
  
18. In **conferma**, fare clic su **configura** per applicare le selezioni e quindi fare clic su **Chiudi**.  
  


