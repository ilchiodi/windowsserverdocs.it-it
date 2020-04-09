---
title: Passaggio 2 pianificare la distribuzione di accesso remoto
description: Questo argomento fa parte della Guida relativa alla gestione remota dei client DirectAccess in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: lizross
author: eross-msft
ms.openlocfilehash: befd517f8e00548524dc9bf9c328d63034c653be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860574"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>Passaggio 2 pianificare la distribuzione di accesso remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver pianificato l'infrastruttura che si intende usare per configurare il singolo server di accesso remoto per la gestione remota dei client DirectAccess, è possibile pianificare le impostazioni che verranno usate dalla configurazione guidata accesso remoto.  
  
> [!NOTE]  
> Prima di continuare con queste attività, vedere [passaggio 1: pianificare l'infrastruttura di accesso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Attività|Descrizione|  
|----|--------|  
|[Pianificare una strategia di distribuzione client](#plan-a-client-deployment-strategy)|Stabilire quali computer gestiti saranno configurati come client DirectAccess.|  
|[Pianificare una strategia di distribuzione del server di accesso remoto](#plan-a-remote-access-server-deployment-strategy)|Pianificare la modalità di distribuzione del server di Accesso remoto.|  
|[Pianificare le configurazioni dei server di infrastruttura](#plan-the-infrastructure-servers-configurations)|Pianificare i server di infrastruttura nella distribuzione di accesso remoto, inclusi il server dei percorsi di rete DirectAccess, i server DNS e i server di Gestione DirectAccess.|  
  
## <a name="plan-a-client-deployment-strategy"></a>Pianificare una strategia di distribuzione client  
Nel pianificare la distribuzione del client sarà necessario prendere tre decisioni:  
  
1.  DirectAccess sarà disponibile solo per i computer portatili o per tutti i computer in un gruppo di sicurezza specifico?  
  
    Quando si configurano i client DirectAccess nell'installazione guidata client DirectAccess, è possibile scegliere di consentire solo ai computer portatili nei gruppi di sicurezza specificati di connettersi al server tramite DirectAccess. Se si limita l'accesso ai computer portatili, Accesso remoto configura automaticamente un filtro WMI per assicurarsi che l'oggetto Criteri di gruppo del client DirectAccess venga applicato solo ai computer portatili nei gruppi di sicurezza specificati. L'amministratore di Accesso remoto richiederà le autorizzazioni per creare o modificare i filtri WMI dei criteri di gruppo per abilitare questa impostazione.  
  
2.  Quali gruppi di sicurezza conterranno i computer client DirectAccess?  
  
    Le impostazioni di DirectAccess sono contenute nell'oggetto Criteri di gruppo del client DirectAccess (GPO). L'oggetto Criteri di gruppo viene applicato ai computer che fanno parte dei gruppi di sicurezza specificati Configurazione guidata client DirectAccess. È possibile specificare i gruppi di sicurezza contenuti in qualsiasi dominio supportato.
  
    Prima di configurare l'accesso remoto, è necessario creare i gruppi di sicurezza. È possibile aggiungere computer al gruppo di sicurezza dopo aver completato la distribuzione di accesso remoto. Tuttavia, se si aggiungono computer client che si trovano in un dominio diverso da quello del gruppo di sicurezza, l'oggetto Criteri di gruppo del client non verrà applicato a tali client. Ad esempio, se è stato creato SG1 nel dominio A per i client DirectAccess e in seguito si aggiungono client dal dominio B a questo gruppo, l'oggetto Criteri di gruppo del client non verrà applicato ai client nel dominio B.  
  
    Per evitare questo problema, creare un nuovo gruppo di sicurezza client per ogni dominio che contiene i computer client. In alternativa, se non si vuole creare un nuovo gruppo di sicurezza, eseguire il cmdlet **Add-daclient** di Windows PowerShell con il nome del nuovo oggetto Criteri di gruppo per il nuovo dominio.  
  
3.  Quali impostazioni vengono configurate per l'assistente connettività di rete DirectAccess?  
  
    L'assistente per la connettività di rete DirectAccess viene eseguito nei computer client e fornisce informazioni aggiuntive sulla connessione DirectAccess agli utenti finali. Nella Configurazione guidata client DirectAccess, è possibile configurare quanto segue:  
  
    -   **Sistemi di verifica della connettività**  
  
        Viene creato probe Web predefinito che i client usano per convalidare la connettività alla rete interna. Il nome predefinito è `https://directaccess-WebProbeHost.<domain_name>`. Il nome deve essere registrato manualmente in DNS. È possibile creare altri sistemi di verifica della connettività che usano altri indirizzi Web su HTTP o PING. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
    -   **Indirizzo di posta elettronica del supporto tecnico**  
  
        Se gli utenti finali riscontrano problemi di connettività DirectAccess, possono inviare un messaggio di posta elettronica contenente informazioni diagnostiche all'amministratore di accesso remoto, che possono risolvere il problema.  
  
    -   **Nome della connessione DirectAccess**  
  
        È possibile specificare un nome di connessione DirectAccess per consentire agli utenti finali di identificare la connessione DirectAccess nel computer.  
  
    -   **Consenti ai client DirectAccess di usare la risoluzione dei nomi locali**  
  
        I client richiedono un mezzo per la risoluzione dei nomi localmente. Se si consente ai client DirectAccess di usare la risoluzione dei nomi locali, gli utenti finali possono usare i server DNS locali per risolvere i nomi. Quando gli utenti finali scelgono di usare i server DNS locali per la risoluzione dei nomi, DirectAccess non invia richieste di risoluzione per i nomi con etichetta singola al server DNS aziendale interno. USA invece la risoluzione dei nomi locali (usando i protocolli LLMNR (Link-Local Multicast Name Resolution) e NetBios su TCP/IP.  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>Pianificare una strategia di distribuzione del server di accesso remoto  
Le decisioni da prendere durante la pianificazione della distribuzione del server di accesso remoto includono:  
  
-   **Topologia di rete**  
  
    Quando si distribuisce un server di accesso remoto, sono disponibili due topologie:  
  
    -   **Due schede**: con due schede di rete, l'accesso remoto può essere configurato con una scheda di rete connessa direttamente a Internet e l'altra connessa alla rete interna. In alternativa, il server viene installato dietro un dispositivo perimetrale, ad esempio un firewall o un router. In questa configurazione, una scheda di rete è connessa alla rete perimetrale e l'altra è connessa alla rete interna.  
  
    -   **Singola scheda di rete**: in questa configurazione il server di accesso remoto viene installato dietro un dispositivo perimetrale, ad esempio un firewall o un router. La scheda di rete viene connessa alla rete interna.  

-   **Schede di rete**  
  
    La configurazione guidata del server di accesso remoto rileva automaticamente le schede di rete configurate nel server di accesso remoto. È necessario assicurarsi di aver selezionato le schede corrette.  
  
-   **Certificato IP-HTTPS**  
  
    La Configurazione guidata del server di Accesso remoto rileva automaticamente un certificato idoneo alla connessione IP-HTTPS. Il nome soggetto del certificato selezionato deve corrispondere all'indirizzo ConnectTo. Se si usano certificati autofirmati, è possibile scegliere di usare un certificato creato automaticamente dal server di accesso remoto.  
  
-   **Prefissi IPv6**  
  
    Se la Configurazione guidata del server di Accesso remoto rileva che IPv6 è stato distribuito sulle schede di rete, vengono automaticamente popolati i prefissi IPv6 per la rete interna, un prefisso IPv6 da assegnare ai computer client DirectAccess e un prefisso IPv6 da assegnare ai computer client VPN. Se i prefissi generati automaticamente non sono corretti per l'infrastruttura IPv6 o ISATAP nativa, sarà necessario modificarli manualmente.  
  
-   **Autenticazione**  
  
    Per autenticare i client DirectAccess nel server di accesso remoto, è possibile scegliere uno dei metodi seguenti:  
  
    -   **Autenticazione utente**: è possibile consentire agli utenti di eseguire l'autenticazione con Active Directory credenziali o con l'autenticazione a due fattori.  
  
    -   **Autenticazione del computer**: è possibile configurare l'autenticazione del computer per l'uso dei certificati. Oppure il server di accesso remoto può fungere da proxy per l'autenticazione Kerberos senza richiedere i certificati. 
  
    -   **Client Windows 7** Per impostazione predefinita, i computer client che eseguono Windows 7 non possono connettersi a una distribuzione di accesso remoto che esegue Windows Server 2012. Se si dispone di client che eseguono Windows 7 nell'organizzazione che richiedono l'accesso remoto alle risorse interne, è possibile consentirne la connessione. Qualsiasi computer client al quale si intende concedere l'accesso alle risorse interne deve essere membro di un gruppo di sicurezza specificato nella Configurazione guidata client DirectAccess.  
  
        > [!NOTE]  
        > Per consentire ai client che eseguono Windows 7 di connettersi tramite DirectAccess, è necessario utilizzare l'autenticazione del certificato del computer.  
  
-   **Configurazione VPN**  
  
    Prima di configurare accesso remoto, decidere se si intende fornire l'accesso VPN ai client remoti. È necessario fornire l'accesso VPN se nell'organizzazione sono presenti computer client che non supportano la connettività DirectAccess (ad esempio, non sono gestiti o eseguono un sistema operativo per il quale DirectAccess non è supportato). L'installazione guidata del server di accesso remoto consente di configurare la modalità di assegnazione degli indirizzi IP (tramite DHCP o da un pool di indirizzi statici) e la modalità di autenticazione dei client VPN (tramite Active Directory o un server RADIUS).  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>Pianificare le configurazioni dei server di infrastruttura  
Accesso remoto richiede tre tipi di server di infrastruttura:  
  
-   **Server del percorso di rete**  
  
-   **Server DNS** 
  
-   **Server di gestione** 
  
## <a name="see-also"></a>Vedere anche  
  
-   [Passaggio 1: pianificare l'infrastruttura di accesso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


