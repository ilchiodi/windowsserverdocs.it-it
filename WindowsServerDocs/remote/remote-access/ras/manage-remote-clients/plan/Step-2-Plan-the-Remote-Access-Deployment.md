---
title: Passaggio 2 pianificare la distribuzione di accesso remoto
description: Questo argomento fa parte della Guida di client DirectAccess di gestire in remoto in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4ad399e62e1aa76b76b6109e28845b2615efa0fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882282"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>Passaggio 2 pianificare la distribuzione di accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo aver pianificato l'infrastruttura che si intende usare per configurare il server di accesso remoto singolo per la gestione remota dei client DirectAccess, si è pronti a pianificare le impostazioni che utilizzerà la configurazione guidata accesso remoto.  
  
> [!NOTE]  
> Prima di continuare con queste attività, vedere [passaggio 1: Pianificare l'infrastruttura di accesso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Attività|Descrizione|  
|----|--------|  
|[Pianificare una strategia di distribuzione client](#bkmk_21client)|Stabilire quali computer gestiti saranno configurati come client DirectAccess.|  
|[Pianificare una strategia di distribuzione di server di accesso remoto](#bkmk_22server)|Pianificare la modalità di distribuzione del server di Accesso remoto.|  
|[Pianificare le configurazioni dei server dell'infrastruttura](#bkmk_23Infservers)|Pianificare i server dell'infrastruttura nella distribuzione di accesso remoto, tra cui il server dei percorsi di rete DirectAccess, server DNS e server di Gestione DirectAccess.|  
  
## <a name="bkmk_21client"></a>Pianificare una strategia di distribuzione client  
Nel pianificare la distribuzione del client sarà necessario prendere tre decisioni:  
  
1.  DirectAccess sarà disponibile ai computer portatili, o a tutti i computer in un gruppo di sicurezza specificato?  
  
    Quando si configurano i client DirectAccess nella configurazione guidata Client DirectAccess, è possibile scegliere di consentire solo ai computer portatili nei gruppi di sicurezza specificato per la connessione al server tramite DirectAccess. Se si limita l'accesso ai computer portatili, Accesso remoto configura automaticamente un filtro WMI per assicurarsi che l'oggetto Criteri di gruppo del client DirectAccess venga applicato solo ai computer portatili nei gruppi di sicurezza specificati. L'amministratore di Accesso remoto richiederà le autorizzazioni per creare o modificare i filtri WMI dei criteri di gruppo per abilitare questa impostazione.  
  
2.  Quali gruppi di sicurezza conterranno i computer client DirectAccess?  
  
    Le impostazioni di DirectAccess sono contenute nel client di DirectAccess oggetti Criteri di gruppo (GPO). L'oggetto Criteri di gruppo viene applicato ai computer che fanno parte dei gruppi di sicurezza specificati Configurazione guidata client DirectAccess. È possibile specificare i gruppi di sicurezza contenuti in qualsiasi dominio supportato.
  
    Prima di configurare accesso remoto, è necessario creare i gruppi di sicurezza. Dopo aver completato la distribuzione di accesso remoto, è possibile aggiungere computer al gruppo di sicurezza. Tuttavia, se si aggiungono computer client che si trovano in un dominio diverso rispetto al gruppo di sicurezza, oggetto Criteri di gruppo client non essere applicato a tutti i client. Ad esempio, se è stato creato SG1 nel dominio A per i client DirectAccess e successivamente si aggiungono i client dal dominio B a questo gruppo, oggetto Criteri di gruppo client verranno ignorati per i client nel dominio B.  
  
    Per evitare questo problema, creare un nuovo gruppo di sicurezza di client per ogni dominio che contiene computer client. In alternativa, se non si desidera creare un nuovo gruppo di sicurezza, eseguire la **Add-DAClient** cmdlet di Windows PowerShell con il nome del nuovo oggetto Criteri di gruppo per il nuovo dominio.  
  
3.  Quali impostazioni vengono configurate per l'Assistente di connettività di rete di DirectAccess?  
  
    L'Assistente di connettività di rete di DirectAccess viene eseguito nei computer client e fornisce informazioni aggiuntive sulla connessione di DirectAccess agli utenti finali. Nella Configurazione guidata client DirectAccess, è possibile configurare quanto segue:  
  
    -   **Strumenti di verifica della connettività**  
  
        Viene creato probe Web predefinito che i client usano per convalidare la connettività alla rete interna. Il nome predefinito è https://directaccess-WebProbeHost.<domain_name>. Il nome deve essere registrato manualmente in DNS. È possibile creare altri strumenti di verifica della connettività che usano altri indirizzi web su HTTP oppure PING. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
    -   **Consentire l'indirizzo di posta elettronica di supporto tecnico**  
  
        Se gli utenti finali riscontrare problemi di connettività DirectAccess, è possibile inviare un messaggio di posta elettronica che contiene le informazioni di diagnostica all'amministratore di accesso remoto, che è possibile risolvere il problema.  
  
    -   **Nome della connessione DirectAccess**  
  
        È possibile specificare un nome della connessione DirectAccess per consentire agli utenti finali di identificare la connessione DirectAccess nei loro computer.  
  
    -   **Consentire ai client DirectAccess di utilizzare la risoluzione dei nomi locali**  
  
        I client richiedono un mezzo per la risoluzione dei nomi in locale. Se si consente ai client DirectAccess di usare la risoluzione dei nomi locali, gli utenti finali possono usare i server DNS locali per risolvere i nomi. Quando gli utenti finali scegliere di usare i server DNS locali per la risoluzione dei nomi, DirectAccess non invia le richieste di risoluzione di nomi con etichetta singola per il server DNS aziendale interno. Usa invece la risoluzione dei nomi locale (usando il Link-Local Multicast Name LLMNR (Resolution) e NetBios su protocolli TCP/IP).  
  
## <a name="bkmk_22server"></a>Pianificare una strategia di distribuzione di server di accesso remoto  
Le decisioni che è necessario apportare quando si prevede di distribuire il server di accesso remoto includono:  
  
-   **Topologia di rete**  
  
    Esistono due topologie disponibili quando si distribuisce un server di accesso remoto:  
  
    -   **Due schede**: Con due schede di rete, accesso remoto può essere configurato con una scheda di rete connessa direttamente a Internet e l'altra connessa alla rete interna. O in alternativa, il server viene installato dietro un dispositivo perimetrale, ad esempio un firewall o un router. In questa rete di una configurazione scheda è connessa alla rete perimetrale e l'altra è connessa alla rete interna.  
  
    -   **Singola scheda di rete**: In questa configurazione accesso remoto server viene installato dietro un dispositivo periferico, ad esempio un firewall o un router. La scheda di rete viene connessa alla rete interna.  

-   **Schede di rete**  
  
    La configurazione guidata Server di accesso remoto rileva automaticamente le schede di rete configurate nel server di accesso remoto. È necessario assicurarsi di aver selezionato le schede corrette.  
  
-   **Certificato IP-HTTPS**  
  
    La Configurazione guidata del server di Accesso remoto rileva automaticamente un certificato idoneo alla connessione IP-HTTPS. Il nome soggetto del certificato selezionato deve corrispondere all'indirizzo ConnectTo. Se si utilizzano certificati autofirmati, è possibile scegliere di usare un certificato che viene creato automaticamente dal server di accesso remoto.  
  
-   **Prefissi IPv6**  
  
    Se la Configurazione guidata del server di Accesso remoto rileva che IPv6 è stato distribuito sulle schede di rete, vengono automaticamente popolati i prefissi IPv6 per la rete interna, un prefisso IPv6 da assegnare ai computer client DirectAccess e un prefisso IPv6 da assegnare ai computer client VPN. Se i prefissi generati automaticamente non sono corretti per l'infrastruttura IPv6 o ISATAP nativa, sarà necessario modificarli manualmente.  
  
-   **Autenticazione**  
  
    È possibile scegliere uno dei seguenti metodi per l'autenticazione dei client DirectAccess al server di accesso remoto:  
  
    -   **L'autenticazione utente**: È possibile abilitare l'autenticazione utente con le credenziali Active Directory o con un'autenticazione a due fattori.  
  
    -   **L'autenticazione computer**: È possibile configurare l'autenticazione del computer per usare i certificati. O il server di accesso remoto può agire come proxy per l'autenticazione Kerberos senza necessità di certificati. 
  
    -   **I client Windows 7** per impostazione predefinita, i computer client che eseguono Windows 7 non è possibile connettersi a una distribuzione di accesso remoto che esegue Windows Server 2012. Se si dispone di client che eseguono Windows 7 nell'organizzazione che richiedono l'accesso remoto alle risorse interne, è possibile consentire loro di connettersi. Qualsiasi computer client al quale si intende concedere l'accesso alle risorse interne deve essere membro di un gruppo di sicurezza specificato nella Configurazione guidata client DirectAccess.  
  
        > [!NOTE]  
        > Consentendo ai client che eseguono Windows 7 per connettersi usando DirectAccess è necessario utilizzare l'autenticazione del certificato computer.  
  
-   **Configurazione VPN**  
  
    Prima di configurare accesso remoto, decidere se si desidera fornire ai client remoti l'accesso VPN. È necessario fornire l'accesso VPN se si dispone di computer client all'interno dell'organizzazione che non supportano la connettività DirectAccess (ad esempio, non sono gestiti o eseguono un sistema operativo per cui DirectAccess non è supportata). La configurazione guidata Server di accesso remoto consente di configurare la modalità di assegnazione degli indirizzi IP (mediante il protocollo DHCP o da un pool di indirizzi statici) e la modalità di autenticazione client VPN (tramite Active Directory o un server RADIUS).  
  
## <a name="bkmk_23Infservers"></a>Pianificare le configurazioni dei server dell'infrastruttura  
Accesso remoto richiede tre tipi di server di infrastruttura:  
  
-   **Server dei percorsi di rete**  
  
-   **Server DNS** 
  
-   **Server di gestione** 
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 1: Pianificare l'infrastruttura di accesso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


