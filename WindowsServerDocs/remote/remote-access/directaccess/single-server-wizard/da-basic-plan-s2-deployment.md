---
title: Passaggio 2 pianificare la distribuzione di base di DirectAccess
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo usando la procedura guidata di Introduzione per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2d9480b3f021fcf1086ca8001997a1c7c1b56456
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308911"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>Passaggio 2 pianificare la distribuzione di base di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver pianificato l'infrastruttura DirectAccess, il passaggio successivo per la distribuzione di DirectAccess in un singolo server con le impostazioni di base consiste nel pianificare le impostazioni per la procedura guidata Introduzione.  
  
|Attività|Descrizione|  
|----|--------|  
|Pianificazione della distribuzione del client|Per impostazione predefinita, la procedura guidata Introduzione distribuisce DirectAccess in tutti i computer portatili e notebook del dominio applicando un filtro WMI all'oggetto Criteri di gruppo delle impostazioni client|  
|Pianificazione della distribuzione del server DirectAccess|Pianificare la modalità di distribuzione del server DirectAccess.|  
  
## <a name="planning-for-client-deployment"></a><a name="bkmk_2_1_client"></a>Pianificazione della distribuzione client  
Nel pianificare la distribuzione del client sarà necessario prendere due decisioni:  
  
1.  DirectAccess sarà disponibile solo ai computer portatili o a qualsiasi computer?  
  
    Quando si configurano i client DirectAccess nella procedura guidata Introduzione, è possibile scegliere di consentire solo ai computer portatili nei gruppi di sicurezza specificati di connettersi tramite DirectAccess. Se si limita l'accesso ai computer portatili, DirectAccess configura automaticamente un filtro WMI per assicurarsi che l'oggetto Criteri di gruppo del client DirectAccess venga applicato solo ai computer portatili nei gruppi di sicurezza specificati. L'amministratore DirectAccess richiede le autorizzazioni per creare o modificare i filtri WMI di criteri di gruppo per abilitare questa impostazione.  
  
2.  Quali gruppi di sicurezza conterranno i computer client DirectAccess?  
  
    Le impostazioni di DirectAccess sono contenute nell'oggetto Criteri di gruppo del client DirectAccess. L'oggetto Criteri di gruppo viene applicato ai computer che fanno parte dei gruppi di sicurezza specificati nella procedura guidata Introduzione. È possibile specificare i gruppi di sicurezza contenuti in qualsiasi dominio supportato. Prima di configurare DirectAccess, è necessario creare i gruppi di sicurezza. È possibile aggiungere computer al gruppo di sicurezza dopo aver completato la distribuzione di DirectAccess. si noti tuttavia che se si aggiungono computer client che si trovano in un dominio diverso al gruppo di sicurezza, l'oggetto Criteri di gruppo del client non verrà applicato a questi client. Ad esempio, se è stato creato SG1 nel dominio A per i client DirectAccess e in seguito si aggiungono i client dal dominio B a questo gruppo, l'oggetto Criteri di gruppo del client non verrà applicato ai client nel dominio B. Per evitare questo problema, creare un nuovo gruppo di sicurezza del client per ciascun dominio che contiene computer client. In alternativa, se non si vuole creare un nuovo gruppo di sicurezza, eseguire il cmdlet Add-DAClient con il nome del nuovo oggetto Criteri di gruppo per il nuovo dominio.  
  
## <a name="planning-for-directaccess-server-deployment"></a><a name="bkmk_2_2_server"></a>Pianificazione della distribuzione del server DirectAccess  
Quando si pianifica la distribuzione del server DirectAccess, è necessario prendere alcune decisioni:  
  
-   **Topologia di rete** : sono disponibili due topologie quando si distribuisce un server DirectAccess:  
  
    -   **Due schede** : con due schede di rete, DirectAccess può essere configurato con una scheda di rete connessa direttamente a Internet e l'altra è connessa alla rete interna. In alternativa, il server viene installato dietro un dispositivo periferico, come un firewall o un router. In questa configurazione, una scheda di rete è connessa alla rete perimetrale e l'altra è connessa alla rete interna.  
  
    -   **Singola scheda di rete** : in questa configurazione il server DirectAccess viene installato dietro un dispositivo perimetrale, ad esempio un firewall o un router. La scheda di rete viene connessa alla rete interna.  
  
-   **Schede di rete** : la procedura guidata DirectAccess rileva automaticamente le schede di rete configurate nel server DirectAccess. È possibile assicurarsi che nella pagina **Revisione** siano selezionate le schede corrette.  
  
-   **Certificato IP-HTTPS** : poiché non è richiesta alcuna infrastruttura PKI in questa distribuzione, la procedura guidata effettua automaticamente il provisioning dei certificati autofirmati per IP-HTTPS e il server dei percorsi di rete (se non sono presenti certificati) e Abilita automaticamente il proxy Kerberos. La procedura guidata Abilita inoltre NAT64 e DNS64 per la conversione del protocollo nell'ambiente solo IPv4. Dopo che la procedura guidata ha applicato la configurazione, fare clic su **Chiudi**.  
  
-   **Client Windows 7** : non è possibile abilitare il supporto per i client Windows 7 dalla procedura guidata introduzione. Questa operazione può essere abilitata dall'installazione guidata avanzata. Per altri dettagli, vedere [distribuire un server DirectAccess singolo con impostazioni avanzate](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
-   **Configurazione VPN** : prima di configurare DirectAccess, decidere se si intende fornire l'accesso VPN ai client remoti. È necessario fornire l'accesso VPN se nell'organizzazione sono presenti computer client che non supportano la connettività DirectAccess, perché non sono gestiti o eseguono un sistema operativo per il quale DirectAccess non è supportato. La procedura guidata Introduzione configura l'assegnazione di indirizzi IP VPN tramite DHCP e configura i client VPN per l'autenticazione tramite Active Directory.  
  
-   **Tunneling forzato** : se si prevede di usare il tunneling forzato o se è possibile aggiungerlo in futuro, è consigliabile usare [Distribuisci un server DirectAccess singolo con impostazioni avanzate](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) per distribuire una configurazione a due tunnel. Per motivi di sicurezza, il tunneling forzato in una configurazione con un solo tunnel non è supportato.  
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>Passaggio precedente  
  
-   [Passaggio 1: pianificare l'infrastruttura DirectAccess di base](da-basic-plan-s1-infrastructure.md)  
  


