---
title: Passaggio 2 pianificare la distribuzione di DirectAccess di base
description: Questo argomento fa parte della Guida di distribuire un Server DirectAccess singolo con l'introduzione avvio procedura guidata per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c82d5e48f26d9defceb3b7583e06eeedbc71a082
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281666"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>Passaggio 2 pianificare la distribuzione di DirectAccess di base

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo aver pianificato l'infrastruttura DirectAccess, il passaggio successivo della distribuzione di DirectAccess in un server singolo con impostazioni di base consiste nel pianificare le impostazioni per la procedura guidata introduttiva.  
  
|Attività|Descrizione|  
|----|--------|  
|Pianificazione della distribuzione del client|Per impostazione predefinita, le attività iniziali guidate consente di distribuire DirectAccess a tutti i computer portatili e notebook nel dominio applicando un filtro WMI a oggetto Criteri di gruppo di impostazioni client|  
|Pianificazione della distribuzione del server DirectAccess|Pianificare la modalità di distribuzione del server DirectAccess.|  
  
## <a name="bkmk_2_1_client"></a>Pianificazione della distribuzione client  
Nel pianificare la distribuzione del client sarà necessario prendere due decisioni:  
  
1.  DirectAccess sarà disponibile solo ai computer portatili o a qualsiasi computer?  
  
    Quando si configurano i client DirectAccess in attività iniziali guidate, è possibile scegliere di consentire solo ai computer portatili nei gruppi di sicurezza specificati di connettersi usando DirectAccess. Se si limita l'accesso ai computer portatili, DirectAccess configura automaticamente un filtro WMI per assicurarsi che il client DirectAccess che viene applicato solo ai computer portatili nei gruppi di sicurezza specificati. L'amministratore di DirectAccess richiede le autorizzazioni per creare o modificare i filtri WMI di criteri di gruppo per abilitare questa impostazione.  
  
2.  Quali gruppi di sicurezza conterranno i computer client DirectAccess?  
  
    Le impostazioni di DirectAccess sono contenute nell'oggetto Criteri di gruppo del client DirectAccess. Oggetto Criteri di gruppo viene applicato ai computer che fanno parte dei gruppi di sicurezza che specificano nella procedura guidata introduttiva. È possibile specificare i gruppi di sicurezza contenuti in qualsiasi dominio supportato. Prima di configurare DirectAccess, i gruppi di sicurezza devono essere creati. È possibile aggiungere computer al gruppo di sicurezza dopo aver completato la distribuzione di DirectAccess, ma si noti che, se si aggiungono computer client che si trovano in un dominio diverso per il gruppo di sicurezza, l'oggetto Criteri di gruppo client verranno non applicato a questi client. Ad esempio, se è stato creato SG1 nel dominio A per i client DirectAccess e in seguito si aggiungono i client dal dominio B a questo gruppo, l'oggetto Criteri di gruppo del client non verrà applicato ai client nel dominio B. Per evitare questo problema, creare un nuovo gruppo di sicurezza del client per ciascun dominio che contiene computer client. In alternativa, se non si vuole creare un nuovo gruppo di sicurezza, eseguire il cmdlet Add-DAClient con il nome del nuovo oggetto Criteri di gruppo per il nuovo dominio.  
  
## <a name="bkmk_2_2_server"></a>Pianificazione della distribuzione del server DirectAccess  
Esistono una serie di decisioni da prendere durante la pianificazione per la distribuzione del server DirectAccess:  
  
-   **Topologia di rete** -esistono due topologie disponibili quando si distribuisce un server DirectAccess:  
  
    -   **Due schede** -con due schede di rete, DirectAccess può essere configurato con una scheda di rete connessa direttamente a Internet e l'altra è connessa alla rete interna. In alternativa, il server viene installato dietro un dispositivo periferico, come un firewall o un router. In questa configurazione, una scheda di rete è connessa alla rete perimetrale e l'altra è connessa alla rete interna.  
  
    -   **Singola scheda di rete** -In questa configurazione DirectAccess server viene installato dietro un dispositivo perimetrale, ad esempio un firewall o un router. La scheda di rete viene connessa alla rete interna.  
  
-   **Schede di rete** -procedura guidata DirectAccess rileva automaticamente le schede di rete configurate nel server DirectAccess. È possibile assicurarsi che le schede corrette vengano selezionate dai **revisione** pagina.  
  
-   **Certificato IP-HTTPS** -perché è presente alcuna infrastruttura a chiave pubblica richiesto in questa distribuzione, la procedura guidata esegue automaticamente il provisioning di certificati autofirmati per IP-HTTPS e il Server dei percorsi rete (se non sono presenti certificati) e abilita automaticamente Proxy Kerberos. La procedura guidata consente anche di NAT64 e DNS64 per la conversione di protocollo nell'ambiente solo IPv4. Dopo che la procedura guidata ha applicato la configurazione, fare clic su **Chiudi**.  
  
-   **I client Windows 7** -non è possibile abilitare il supporto per i client Windows 7 da attività iniziali guidate. È possibile abilitarla dalla configurazione guidata avanzata. Per altre informazioni, vedere [distribuire un DirectAccess Server singolo con impostazioni avanzate](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
-   **Configurazione VPN** -prima di configurare DirectAccess, decidere se si desidera fornire ai client remoti l'accesso VPN. È necessario fornire l'accesso VPN se si dispone di computer client all'interno dell'organizzazione che non supportano la connettività DirectAccess (in quanto non sono gestiti o eseguono un sistema operativo per cui DirectAccess non è supportata). Attività iniziali guidate Configura Assegnazione indirizzo IP VPN Usa DHCP e configura i client VPN per essere autenticato usando Active Directory.  
  
-   **Forzare il Tunneling** -se si prevede di usare il Tunneling forzato, o si pensa di aggiungerlo in futuro, è necessario utilizzare [distribuire un DirectAccess Server singolo con impostazioni avanzate](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) per distribuire una configurazione a due tunnel. A causa di considerazioni sulla sicurezza, il Tunneling forzato in una configurazione del tunnel singolo non è supportato.  
  
## <a name="BKMK_Links"></a>Passaggio precedente  
  
-   [Passaggio 1: Pianificare l'infrastruttura di base di DirectAccess](da-basic-plan-s1-infrastructure.md)  
  


