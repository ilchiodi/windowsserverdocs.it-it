---
title: Funzionalità VPN Always On
description: In questo argomento, scoprire le funzionalità di VPN Always On.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066855"
---
# Caratteristiche e funzionalità VPN always On

>Si applica a: Windows Server \(Semi-Annual Channel\), Windows Server 2016, Windows 10

& #171;  [ **Precedente:** sempre nella distribuzione di VPN per Windows Server e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
& #187; [ **Successivo:** Scopri i miglioramenti VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

In questo argomento, scoprire le funzionalità e caratteristiche di VPN Always On.  La tabella seguente non è un elenco esaustivo, tuttavia, include alcuni dei più comuni caratteristiche e usati nelle soluzioni di accesso remoto. 

>[!TIP]
>Se attualmente usi DirectAccess, ti consigliamo di esaminare la funzionalità VPN Always On con attenzione per determinare se si occupa di che tutte le esigenze di accesso remoto prima della migrazione di formare DirectAccess di VPN Always On.  

| Area funzionale | VPN Always On  |
| ---- | ---- |
| Aspetto semplice e trasparente la connettività alla rete aziendale. | È possibile configurare VPN Always On per supportare l'attivazione automatica in base alle richieste di risoluzione dello spazio dei nomi o di avvio dell'applicazione.<p><p>Definire mediante:<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName//domainnameinformationlist/AutoTrigger** |
| Utilizzo di un Tunnel dell'infrastruttura dedicata per fornire la connettività per gli utenti non firmati nella rete aziendale. | Puoi ottenere questa funzionalità tramite la funzionalità di Tunnel dei dispositivi nel profilo della VPN.<p><p>_**Nota.**_<br>Tunnel dei dispositivi possa essere configurato solo nei dispositivi aggiunti al dominio utilizzando IKEv2 con autenticazione di certificati del computer.<p><p>Definire mediante:<br>**VPNv2/ProfileName/DeviceTunnel** |
| Utilizzo di gestire scalabilità orizzontale per consentire la connettività remota ai client da sistemi di gestione che si trova nella rete aziendale. | Puoi ottenere questa funzionalità tramite la funzionalità di Tunnel dei dispositivi nel profilo della VPN, in combinazione con la configurazione della connessione VPN per registrare in modo dinamico gli indirizzi IP assegnati all'interfaccia VPN con i servizi DNS interni.<p><p>_**Nota.**_<br>Se riattivi filtri del traffico nel profilo al Tunnel dei dispositivi, il Tunnel dei dispositivi Nega il traffico in ingresso (dalla rete aziendale nel client).<p><p>Definire mediante:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| Rientrano nuovamente quando i client si trovano dietro firewall o server proxy. | È possibile configurare per eseguire il fallback a SSTP (da IKEv2) usando il tipo di tunnel/protocollo automatica all'interno del profilo VPN.<p><p>_**Nota.**_<br>Tunnel utente supporta il protocollo SSTP e IKEv2 e Tunnel dei dispositivi supporta IKEv2 solo con nessun supporto per il fallback SSTP.<p>Definire mediante:<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| Supporto per la modalità di accesso end-to-edge. | VPN Always On consente la connettività alle risorse aziendali usando criteri di tunnel che richiedono l'autenticazione e crittografia fino a raggiungere il gateway VPN. Per impostazione predefinita, le sessioni di tunnel terminano in corrispondenza il gateway VPN, che funge anche dal gateway IKEv2, fornendo la sicurezza end-to-edge. |
| Supporto per l'autenticazione del certificato computer. | Il tipo di protocollo IKEv2 disponibile come parte della piattaforma VPN Always On supporta in modo specifico l'uso dei certificati computer o un computer per l'autenticazione VPN.<p><p>_**Nota.**_<br>IKEv2 è l'unico protocollo supportato per Tunnel dei dispositivi e vi è alcuna possibilità di supporto per il fallback SSTP. <p>Definire mediante:<br>**VPNv2/ProfileName/NativeProfile/autenticazione/MachineMethod** |
| Utilizzare i gruppi di sicurezza per limitare le funzionalità di accesso remoto ai client specifici. | È possibile configurare VPN Always On per supportare l'autorizzazione granulare quando si utilizza raggio, che include l'uso di gruppi di sicurezza per controllare l'accesso VPN. |
| Supporto per i server dietro un firewall edge o un dispositivo NAT. | VPN Always On offre la possibilità di usare protocolli come IKEv2 e il protocollo SSTP che supportano l'uso di un gateway VPN che si trova dietro un firewall di dispositivo o edge NAT.<p><p>_**Nota.**_<br>Tunnel utente supporta il protocollo SSTP e IKEv2 e Tunnel dei dispositivi supporta IKEv2 solo con nessun supporto per il fallback SSTP. |
| Possibilità di determinare la connettività intranet quando è connesso alla rete aziendale. | Rilevamento della rete attendibile offre la possibilità di rilevare le connessioni di rete aziendale e si basa su una valutazione del suffisso DNS specifico della connessione assegnato a interfacce di rete e profilo di rete.<p><p>Definire mediante:<br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| Conformità con protezione accesso (alla rete). | Il client VPN Always On può essere integrato con accesso condizionale di Azure per applicare la conformità dei dispositivi, MFA o una combinazione di entrambi. Quando conformi ai criteri di accesso condizionale, Azure AD emette un certificato di autenticazione IPsec breve (per impostazione predefinita, 60 minuti) che il client può quindi usare per l'autenticazione al gateway VPN. Conformità dei dispositivi sfrutta i criteri di conformità di System Center Configuration Manager/Intune, che possono includere lo stato attestazione dell'integrità del dispositivo. In questo momento, accesso condizionale di Azure VPN offre la sostituzione più vicina alla soluzione esistente protezione accesso alla rete, anche se non esiste alcuna forma di correzione servizio o il quarantena funzionalità di rete. Per ulteriori informazioni, vedi [VPN e accesso condizionale](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access).<p>Definire mediante:<br>**/ ProfileName/devicecompliance del CSP VPNv2** |
| Possibilità di definire i server di gestione sono accessibili prima di accesso utente in. | È possibile ottenere questa funzionalità in VPN Always On tramite la funzionalità di Tunnel dei dispositivi (disponibile nella versione 1709 – per solo IKEv2) nel profilo della VPN, combinato con i filtri di traffico per controllare quali sistemi di gestione della rete aziendale sono accessibili tramite il Tunnel dei dispositivi.<p><p>_**Nota.**_<br>Se riattivi filtri del traffico nel profilo al Tunnel dei dispositivi, il Tunnel dei dispositivi Nega il traffico in ingresso (dalla rete aziendale nel client).<p>Definire mediante:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## Funzionalità aggiuntive

Ogni elemento in questa sezione è un uso scenario maiuscole o funzionalità di accesso remoto usati comunemente per cui VPN Always On ha migliorato funzionalità, ovvero tramite un'espansione della funzionalità o l'eliminazione di una limitazione precedente.


| Area funzionale | VPN Always On  |
| ---- | ---- |
| Dispositivi con Enterprise SKU requisito appartenenti al dominio. | Supporta VPN Always On appartenenti al dominio, appartenenti a non di dominio (gruppo di lavoro) o AD – dispositivi aggiunti ad Azure per consentire per gli scenari BYOD e aziendali. VPN Always On è disponibile in tutte le edizioni di Windows e le funzionalità della piattaforma sono disponibili a terze parti tramite il supporto del plug-in VPN UWP.<p><p>_**Nota.**_<br>Tunnel dei dispositivi possa essere configurato solo nei dispositivi aggiunti al dominio che esegue Windows 10 Enterprise o Education versione 1709 o successiva. Non esiste alcun supporto per il controllo di terze parti del Tunnel dei dispositivi. |
| Supporto per IPv4 e IPv6. | Con VPN Always On, gli utenti possono accedere alle risorse IPv4 e IPv6 nella rete aziendale. Il client VPN Always On Usa un approccio dual-stack che in modo specifico non si basi sulla IPv6 o la necessità del gateway VPN per fornire servizi di traduzione NAT64 o DNS64. |
| Supporto per l'autenticazione OTP o a due fattori. |La piattaforma VPN Always On in modo nativo supporta il protocollo EAP, che consente l'uso di tipi EAP di terze parti e diversificata Microsoft come parte del flusso di lavoro autenticazione. VPN Always On supporta in modo specifico della smart card (fisica e virtuale) e Windows Hello per i certificati di Business per soddisfare i requisiti di autenticazione a due fattori. Inoltre, VPN Always On supporta OTP tramite MFA (non supportato in modalità nativa, solo supportato nel plug-in di terze parti) tramite l'integrazione raggio EAP.<p><p>Definire mediante:<br>**VPNv2/ProfileName/NativeProfile/autenticazione** |
| Supporto per più domini e foreste. | La piattaforma VPN Always On non presenta alcuna dipendenza topologia di foreste o di dominio Active Directory Domain Services (AD DS) (o livelli funzionali/schema associato) perché non richiede il client VPN per essere aggiunto alla funzione di dominio. Criteri di gruppo non sono pertanto una dipendenza per definire le impostazioni del profilo VPN perché non usarlo durante la configurazione client. In cui è necessaria l'integrazione di autorizzazione di Active Directory, puoi ottenere attraverso raggio come parte del processo di autenticazione e autorizzazione EAP. |
| Supporto per entrambi split e imposizione del tunneling per la separazione di traffico internet o intranet. | È possibile configurare VPN Always On per supportare entrambi con imposizione del tunneling (modalità operativa l'impostazione predefinita) e split tunneling in modalità nativa. VPN Always On offre maggiore granularità per i criteri di routing specifiche dell'applicazione.<p><p>_**Nota.**_<br>Supportato da solo Tunnel utente.<p><p>Definire mediante:<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/App/RoutingPolicyType** |
| Supporto di più protocolli. | VPN Always On può essere configurato per supportare il protocollo SSTP in modalità nativa se Secure Sockets Layer fallback da IKEv2 è necessaria.<p><p>_**Nota.**_<br>Tunnel utente supporta il protocollo SSTP e IKEv2 e Tunnel dei dispositivi supporta IKEv2 solo con nessun supporto per il fallback SSTP.  |
| Connettività Assistente per fornire lo stato della connettività aziendale. | VPN Always On completamente integrato con nativo Network Connectivity Assistant e fornisce lo stato della connettività dall'interfaccia di visualizzazione tutte le reti. Con l'avvento di Windows 10 Creators Update (versione 1703), lo stato delle connessioni VPN e il controllo della connessione VPN per utente Tunnel sono ora disponibili tramite il riquadro a comparsa di rete (per Windows predefinite client VPN), nonché. |
| Risoluzione dei nomi delle risorse aziendale usando-nome breve, il nome di dominio completo (FQDN) e il suffisso DNS. | VPN Always On in modalità nativa definire uno o più suffissi DNS come parte del processo di assegnazione indirizzo IP, tra cui la risoluzione dei nomi brevi, FQDN o intero spazi dei nomi DNS delle risorse aziendali e di connessione VPN. VPN Always On supporta anche l'uso delle tabelle criteri di risoluzione dei nomi per fornire una granularità al livello di risoluzione dei nomi specifico.<p><p>_**Nota.**_<br>Evita l'uso dei suffissi globale come che interferiscono con risoluzione shortname quando usi le tabelle di criteri di risoluzione dei.<p><p>Definire mediante:<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName//domainnameinformationlist** |
---



## Passaggi successivi

- [Altre informazioni sui miglioramenti apportati VPN Always On](always-on-vpn/always-on-vpn-enhancements.md)

- [Scopri alcune delle funzionalità VPN Always On avanzate](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [Altre informazioni sulla tecnologia di VPN Always On](always-on-vpn/always-on-vpn-technology-overview.md)

- [Iniziare a pianificare la distribuzione di VPN Always On](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
