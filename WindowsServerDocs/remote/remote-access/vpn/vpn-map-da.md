---
title: Funzionalità VPN Always On
description: In questo argomento illustra le funzionalità di VPN Always On.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874812"
---
# <a name="always-on-vpn-features-and-functionalities"></a>Le funzionalità e le funzionalità di VPN always On

>Si applica a: Windows Server \(canale semestrale\), Windows Server 2016, Windows 10

&#171;  [**Precedente:** Sempre nella distribuzione VPN per Windows Server e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
&#187; [ **Next:** Scopri i miglioramenti di VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

In questo argomento illustra le caratteristiche e funzionalità di VPN Always On.  Nella tabella seguente non è un elenco completo, tuttavia, sono incluse alcune delle funzionalità e le funzionalità usate nelle soluzioni di accesso remoto più comuni. 

>[!TIP]
>Se attualmente si utilizza DirectAccess, è consigliabile prendere in considerazione le funzionalità di VPN Always On attentamente per determinare se punta a che tutte le esigenze di accesso remoto prima della migrazione di formare DirectAccess a VPN Always On.  

| Area funzionale | VPN Always On  |
| ---- | ---- |
| Trasparente, trasparente per la connettività alla rete aziendale. | È possibile configurare VPN Always On per supportare l'attivazione automatica in base le richieste di risoluzione lancio o spazio dei nomi dell'applicazione.<p><p>Definire usando:<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName/DomainNameInformationList/AutoTrigger** |
| Utilizzo di un Tunnel di infrastruttura dedicati per fornire connettività per gli utenti non connessi alla rete aziendale. | È possibile ottenere questa funzionalità usando la funzionalità di dispositivo Tunnel nel profilo VPN.<p><p>_**Si noti.**_<br>Dispositivo Tunnel può essere configurata solo nei dispositivi appartenenti a un dominio con l'autenticazione del certificato computer IKEv2.<p><p>Definire usando:<br>**VPNv2/ProfileName/DeviceTunnel** |
| Utilizzo di gestione di istanze per consentire la connettività remota per i client dai sistemi di gestione che si trova nella rete aziendale. | È possibile ottenere questa funzionalità usando la funzionalità di dispositivo Tunnel nel profilo VPN combinato con la configurazione della connessione VPN per registrare in modo dinamico gli indirizzi IP assegnati all'interfaccia VPN con i servizi DNS interni.<p><p>_**Si noti.**_<br>Se si attiva i filtri traffico nel profilo di dispositivo Tunnel, il Tunnel di dispositivo rifiuta il traffico in ingresso (dalla rete aziendale al client).<p><p>Definire usando:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| Fallback quando i client sono protetti da firewall o server proxy. | È possibile configurare per eseguire il fallback a SSTP (da IKEv2) utilizzando il tipo di protocollo o tunnel automatico nel profilo VPN.<p><p>_**Si noti.**_<br>Utente Tunnel supporta SSTP e IKEv2 e Tunnel dispositivo supporta IKEv2 solo con alcun supporto per il fallback a SSTP.<p>Definire usando:<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| Supporto per la modalità di accesso end-to-edge. | VPN Always On fornisce la connettività alle risorse aziendali usando i criteri del tunnel che richiedono l'autenticazione e crittografia fino a raggiungere il gateway VPN. Per impostazione predefinita, le sessioni di tunnel terminano con il gateway VPN, che funge anche da gateway IKEv2, fornendo sicurezza end-to-edge. |
| Supporto per l'autenticazione del certificato computer. | Il tipo di protocollo IKEv2 disponibile come parte della piattaforma VPN Always On supporta in modo specifico l'uso di certificati di computer o del computer per l'autenticazione VPN.<p><p>_**Si noti.**_<br>IKEv2 è l'unico protocollo supportato per il Tunnel di dispositivo e non sono disponibili opzioni di supporto per il fallback a SSTP. <p>Definire usando:<br>**VPNv2/ProfileName/NativeProfile/Authentication/MachineMethod** |
| Usare i gruppi di sicurezza per limitare le funzionalità di accesso remoto a client specifici. | È possibile configurare VPN Always On per supportare l'autorizzazione granulare quando si utilizza RADIUS, che include l'uso di gruppi di sicurezza per controllare l'accesso VPN. |
| Supporto per i server dietro un firewall periferico o un dispositivo NAT. | VPN Always On offre la possibilità di usare protocolli quali IKEv2 e SSTP che supportano completamente l'uso di un gateway VPN che si trova dietro un firewall NAT, dispositivo o edge.<p><p>_**Si noti.**_<br>Utente Tunnel supporta SSTP e IKEv2 e Tunnel dispositivo supporta IKEv2 solo con alcun supporto per il fallback a SSTP. |
| Possibilità di determinare la connettività intranet quando si è connessi alla rete aziendale. | Rilevamento della rete attendibile offre la possibilità di rilevare le connessioni di rete aziendale e si basa su una valutazione del suffisso DNS specifico della connessione assegnato al profilo di rete e le interfacce di rete.<p><p>Definire usando:<br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| Conformità con protezione accesso rete (NAP). | Il client VPN Always On può integrarsi con accesso condizionale di Azure per imporre l'autenticazione a più fattori, la conformità dei dispositivi o una combinazione di entrambi. Quando si è conforme ai criteri di accesso condizionale, Azure AD rilascia un certificato di autenticazione IPsec temporanea (per impostazione predefinita, 60 minuti) che il client può quindi utilizzare per l'autenticazione per il gateway VPN. Conformità del dispositivo consente di sfruttare i criteri di conformità di System Center Configuration Manager o Intune, che possono includere lo stato di attestazione dell'integrità di dispositivo. A questo punto, l'accesso condizionale di VPN di Azure fornisce la sostituzione più vicina alla soluzione di protezione accesso alla rete esistente, anche se non esiste alcuna forma di monitoraggio e aggiornamento funzionalità di rete del servizio o la quarantena. Per altre informazioni, vedere [VPN e l'accesso condizionale](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access).<p>Definire usando:<br>**VPNv2/ProfileName/DeviceCompliance** |
| Possibilità di definire quali server di gestione siano accessibili prima dell'accesso dell'utente. | È possibile ottenere questa funzionalità nella VPN Always On usando la funzionalità di dispositivo Tunnel (disponibile in versione 1709 – per IKEv2 solo) nel profilo VPN combinato con i filtri traffico per controllare quali sistemi di gestione nella rete aziendale sono accessibili tramite il Dispositivo Tunnel.<p><p>_**Si noti.**_<br>Se si attiva i filtri traffico nel profilo di dispositivo Tunnel, il Tunnel di dispositivo rifiuta il traffico in ingresso (dalla rete aziendale al client).<p>Definire usando:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## <a name="additional-functionalities"></a>Funzionalità aggiuntive

Ogni elemento in questa sezione è un scenario d'uso o funzionalità di uso comune di accesso remoto per il quale VPN Always On ha migliorato la funzionalità, ovvero tramite un'espansione della funzionalità o l'eliminazione di un limite precedente.


| Area funzionale | VPN Always On  |
| ---- | ---- |
| Dispositivi aggiunti a un dominio con SKU Enterprise requisito. | Supporta VPN Always On appartenenti a un dominio, aggiunto non di dominio (gruppo di lavoro) o Active Directory-dispositivi aggiunti ad Azure per consentire enterprise sia gli scenari BYOD. VPN Always On è disponibile in tutte le edizioni di Windows e le funzionalità della piattaforma sono disponibili a terze parti tramite il supporto del plug-in VPN di piattaforma UWP.<p><p>_**Si noti.**_<br>Dispositivo Tunnel può essere configurata solo nei dispositivi appartenenti a un dominio che eseguono Windows 10 Enterprise o Education versione 1709 o successiva. Non vi è alcun supporto per il controllo di terze parti del Tunnel della periferica. |
| Supporto per IPv4 e IPv6. | Con VPN Always On, gli utenti possono accedere alle risorse IPv4 e IPv6 nella rete aziendale. Il client VPN Always On utilizza un approccio dual stack che non specificamente dipende IPv6 o la necessità per il gateway VPN per fornire servizi di traduzione NAT64 o DNS64. |
| Supporto per l'autenticazione OTP o a due fattori. |La piattaforma VPN Always On supporta in modo nativo EAP, che consente l'uso di Microsoft diversi e i tipi EAP di terze parti come parte del flusso di lavoro autenticazione. VPN Always On supporta in modo specifico la smart card (fisiche e virtuali) e Windows Hello per i certificati di Business per soddisfare i requisiti di autenticazione a due fattori. VPN Always On supporta inoltre OTP tramite autenticazione a più fattori (non supportato in modalità nativa, supportato nel plug-in di terze parti solo) tramite integrazione RADIUS EAP.<p><p>Definire usando:<br>**VPNv2/ProfileName/NativeProfile/Authentication** |
| Supporto per più domini e foreste. | La piattaforma VPN Always On non ha dipendenze sulla topologia di insiemi di strutture o dominio di Active Directory Domain Services (AD DS) (o livelli funzionali/schema associato) perché non richiede il client VPN appartenere al dominio alla funzione. Criteri di gruppo non sono pertanto una dipendenza per definire le impostazioni del profilo VPN perché non usarlo durante la configurazione del client. In cui è richiesta l'integrazione di autorizzazione di Active Directory, è possibile ottenere, tramite RADIUS come parte del processo di autenticazione e autorizzazione di EAP. |
| Supporto per entrambi split e forza tunnel per separare il traffico internet/intranet. | È possibile configurare VPN Always On per supportare entrambi forza il tunnel (la modalità operativa predefinita) e split tunneling, in modo nativo. VPN Always On fornisce maggiore granularità per i criteri di routing specifico dell'applicazione.<p><p>_**Si noti.**_<br>Supportato da solo utente Tunnel.<p><p>Definire usando:<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/App/RoutingPolicyType** |
| Supporto di più protocolli. | VPN Always On possono essere configurati per supportare in modo nativo SSTP se Secure Sockets Layer fallback dal IKEv2 è obbligatorio.<p><p>_**Si noti.**_<br>Utente Tunnel supporta SSTP e IKEv2 e Tunnel dispositivo supporta IKEv2 solo con alcun supporto per il fallback a SSTP.  |
| Assistente connettività di fornire lo stato della connettività aziendale. | VPN Always On è completamente integrato con il nativo Assistente connettività di rete e fornisce lo stato di connettività dall'interfaccia Visualizza tutte le reti. Con l'avvento di Windows 10 Creators Update (versione 1703), lo stato della connessione VPN e il controllo della connessione VPN per utente Tunnel sono ora disponibili tramite il riquadro a comparsa di rete (per Windows incorporato client VPN), nonché. |
| Risoluzione dei nomi delle risorse aziendali usando breve-name, nome di dominio completo (FQDN) e il suffisso DNS. | VPN Always On in modo nativo possibile definire uno o più suffissi DNS come parte del processo di assegnazione indirizzo IP, tra cui la risoluzione dei nomi di risorsa aziendale per i nomi brevi, FQDN o interi spazi dei nomi DNS e connessione VPN. VPN Always On supporta anche l'utilizzo delle tabelle di criteri di risoluzione nome per fornire la granularità di risoluzione dello spazio dei nomi specifico.<p><p>_**Si noti.**_<br>Evitare l'uso di suffissi globale come che interferiscano con la risoluzione dei nome breve, quando si usano criteri risoluzione nomi tabelle.<p><p>Definire usando:<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName/DomainNameInformationList** |
---



## <a name="next-steps"></a>Passaggi successivi

- [Altre informazioni sui miglioramenti VPN Always On](always-on-vpn/always-on-vpn-enhancements.md)

- [Informazioni su alcune delle funzionalità avanzate VPN Always On](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [Altre informazioni relative alle tecnologie VPN Always On](always-on-vpn/always-on-vpn-technology-overview.md)

- [Iniziare a pianificare la distribuzione VPN Always On](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
