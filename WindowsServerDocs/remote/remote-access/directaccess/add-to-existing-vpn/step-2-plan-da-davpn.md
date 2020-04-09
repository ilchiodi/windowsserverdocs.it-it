---
title: Passaggio 2 pianificare la distribuzione di DirectAccess
description: Questo argomento fa parte della guida aggiungere DirectAccess a una distribuzione di accesso remoto esistente (VPN) per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 72b5b2af-6925-41e0-a3f9-b8809ed711d1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 98fb738f3535845a2117f2f6547856b9081bd7d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859544"
---
# <a name="step-2-plan-the-directaccess-deployment"></a>Passaggio 2 pianificare la distribuzione di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver pianificato l'infrastruttura di Accesso remoto, il passaggio successivo per abilitare DirectAccess consiste nel pianificare le impostazioni per l'Abilitazione guidata DirectAccess.  
  
|Attività|Descrizione|  
|----|--------|  
|Pianificazione della distribuzione del client|Pianificare la modalità di connessione dei computer client con DirectAccess. Stabilire quali computer gestiti saranno configurati come client DirectAccess.|  
|Pianificazione della distribuzione del server di Accesso remoto|Pianificare la modalità di distribuzione del server di Accesso remoto.|  
  
## <a name="planning-for-client-deployment"></a><a name="bkmk_2_1_client"></a>Pianificazione della distribuzione client  
Nel pianificare la distribuzione del client sarà necessario prendere due decisioni:  
  
-   DirectAccess sarà disponibile solo ai computer portatili o a qualsiasi computer?  
  
    Quando si configurano i client DirectAccess nell'Abilitazione guidata DirectAccess è possibile scegliere se consentire solo ai computer portatili nei gruppi di sicurezza specificati di connettersi con DirectAccess. Se si limita l'accesso ai computer portatili, Accesso remoto configura automaticamente un filtro WMI per assicurarsi che l'oggetto Criteri di gruppo del client DirectAccess venga applicato solo ai computer portatili nei gruppi di sicurezza specificati. L'amministratore di Accesso remoto richiederà le autorizzazioni per creare o modificare i filtri WMI dei criteri di gruppo per abilitare questa impostazione.  
  
-   Quali gruppi di sicurezza conterranno i computer client DirectAccess?  
  
    Le impostazioni di DirectAccess sono contenute nell'oggetto Criteri di gruppo del client DirectAccess. L'oggetto Criteri di gruppo viene applicato ai computer che fanno parte dei gruppi di sicurezza specificati nell'Abilitazione guidata DirectAccess. È possibile specificare i gruppi di sicurezza contenuti in qualsiasi dominio supportato. Prima di configurare Accesso remoto, occorre creare i gruppi di sicurezza. È possibile aggiungere computer al gruppo di sicurezza dopo aver completato la distribuzione di Accesso remoto, ma si tenga presente che se si aggiungono computer client residenti in un dominio differente dal gruppo di sicurezza, l'oggetto Criteri di gruppo del client non verrà applicato a questi client. Ad esempio, se è stato creato SG1 nel dominio A per i client DirectAccess e in seguito si aggiungono i client dal dominio B a questo gruppo, l'oggetto Criteri di gruppo del client non verrà applicato ai client nel dominio B. Per evitare questo problema, creare un nuovo gruppo di sicurezza del client per ciascun dominio che contiene computer client. In alternativa, se non si vuole creare un nuovo gruppo di sicurezza, eseguire il cmdlet Add-DAClient con il nome del nuovo oggetto Criteri di gruppo per il nuovo dominio.  
  
## <a name="planning-for-remote-access-server-deployment"></a><a name="bkmk_2_2_server"></a>Pianificazione della distribuzione del server di accesso remoto  
Nel pianificare la distribuzione del server di Accesso remoto occorre prendere alcune decisioni:  
  
-   **Topologia di rete**: sono disponibili due topologie quando si distribuisce un server di accesso remoto:  
  
    -   **Due schede**: con due schede di rete, l'accesso remoto può essere configurato con una scheda di rete connessa direttamente a Internet e l'altra è connessa alla rete interna. In alternativa, il server viene installato dietro un dispositivo periferico, come un firewall o un router. In questa configurazione, una scheda di rete è connessa alla rete perimetrale e l'altra è connessa alla rete interna.  
  
    -   **Singola scheda di rete**: in questa configurazione il server di accesso remoto viene installato dietro un dispositivo perimetrale, ad esempio un firewall o un router. La scheda di rete viene connessa alla rete interna.  
  
-   **Schede di rete**: l'abilitazione guidata DirectAccess rileva automaticamente le schede di rete configurate nel server di accesso remoto, in base alle interfacce usate dalla VPN. È necessario assicurarsi di aver selezionato le schede corrette.  
  
-   **Indirizzo ConnectTo**-i computer client usano l'indirizzo ConnectTo per connettersi al server di accesso remoto. L'indirizzo scelto dall'utente deve corrispondere al nome soggetto del certificato IP-HTTPS distribuito per la connessione IP-HTTPS ed essere disponibile nel DNS pubblico. Vedere Pianificare i certificati per IP-HTTPS  
  
-   **Certificato IP-HTTPS**-se è configurata la VPN SSTP, l'abilitazione guidata DirectAccess preleva il certificato usato da SSTP per IP-HTTPS. Se la connessione VPN SSTP non è configurata, l'Abilitazione guidata DirectAccess tenterà di verificare se è stato configurato un certificato per IP-HTTPS. In caso contrario, verrà effettuato il provisioning automatico dei certificati autofirmati per IP-HTTPS. La procedura guidata abilita automaticamente l'autenticazione Kerberos. oltre a NAT64 e DNS64 per la traduzione dei protocolli nell'ambiente solo IPv4.  
  
-   **Prefissi IPv6**: se la procedura guidata rileva che IPv6 è stato distribuito sulle schede di rete, crea automaticamente i prefissi IPv6 per la rete interna, un prefisso IPv6 da assegnare ai computer client DirectAccess e un prefisso IPv6 da assegnare ai computer client VPN. Se i prefissi generati automaticamente non sono corretti per l'infrastruttura IPv6 o ISATAP nativa, sarà necessario modificarli manualmente. Vedere 1.1 Pianificare la topologia e le impostazioni di rete e server.  
  
-   **Client Windows 7**: per impostazione predefinita, i computer client Windows 7 non possono connettersi a una distribuzione di accesso remoto di windows Server 2012. Se nell'organizzazione sono presenti computer client Windows 7 che richiedono l'accesso remoto alle risorse interne, è possibile consentirne la connessione. Qualsiasi computer client al quale si intende concedere l'accesso alle risorse interne deve essere membro di un gruppo di sicurezza specificato nell'Abilitazione guidata DirectAccess.  
  
    > [!NOTE]
    > Per consentire ai computer client Windows 7 di connettersi utilizzando DirectAccess, è necessario utilizzare l'autenticazione del certificato computer.
  
-   **Autenticazione**: l'abilitazione guidata directaccess usa Active Directory per autenticare le credenziali utente. Per distribuire l'autenticazione a due fattori. vedere [Distribuire Accesso remoto con l'autenticazione OTP](../../ras/otp/Deploy-RA-OTP.md).  
  

  


