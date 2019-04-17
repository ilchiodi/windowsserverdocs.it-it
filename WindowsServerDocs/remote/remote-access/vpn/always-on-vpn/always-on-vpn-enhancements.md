---
title: Miglioramenti di VPN Always On
description: VPN Always On offre molti vantaggi rispetto alle soluzioni VPN di Windows di passato. Miglioramenti principali di integrazione, sicurezza, connettività, controllo di rete e compatibilità allineano VPN Always On visione di Microsoft cloud-first, incentrato sui dispositivi mobili.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067341"
---
# Miglioramenti di VPN Always On

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows 10

& #171; [ **Precedente:** informazioni sulle funzionalità VPN Always On](../vpn-map-da.md)<br>
& #187; [ **Successivo:** ulteriori informazioni sulla tecnologia di VPN Always On](always-on-vpn-technology-overview.md)

VPN Always On offre molti vantaggi rispetto alle soluzioni VPN di Windows di passato. I seguenti miglioramenti principali allineati VPN Always On visione di Microsoft cloud-first, incentrato sui dispositivi mobili:

- **Integrazione con la piattaforma:** VPN Always On ha migliore integrazione con il sistema operativo Windows e le soluzioni di terze parti per fornire una piattaforma affidabile per innumerevoli scenari avanzati di connessione.

- **Sicurezza:** VPN Always On include funzionalità di sicurezza di nuovo, avanzato per limitare il tipo di traffico, quali applicazioni possono usare la connessione VPN, e i metodi di autenticazione è possibile utilizzare per avviare la connessione. Quando la connessione è attiva la maggior parte dei casi, è particolarmente importante proteggere la connessione. Per ulteriori informazioni, vedere le [Opzioni di autenticazione VPN](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication).

- **La connettività VPN:** VPN Always On, con o senza Tunnel dei dispositivi offre funzionalità di attivazione automatica. Prima di VPN Always On, la possibilità di attivare una connessione automatica tramite utente o l'autenticazione del dispositivo non era possibile.  

- **Controllo di rete:** VPN Always On consente agli amministratori di specificare i criteri di routing a un livello più granulare, ovvero anche verso il basso per l'applicazione singole, ovvero che è ideale per le app line-of-business (LOB) che richiedono l'accesso remoto speciale.  VPN Always On anche è completamente compatibile con entrambi Internet Protocol versione 4 (IPv4) e la versione 6 (IPv6). A differenza di DirectAccess, non esiste alcuna dipendenza specifico su IPv6.

  >[!Note]
  >Prima di iniziare, assicurati di abilitare IPv6 sul server VPN. In caso contrario, non può essere stabilita una connessione e visualizza un messaggio di errore.
  
- **Configurazione e compatibilità:** VPN Always On possono essere distribuite e gestite diversi modi, che offre numerosi vantaggi rispetto gli altri software VPN client VPN Always On.

## Integrazione con la piattaforma

Microsoft ha introdotto o migliorata le seguenti funzionalità di integrazione in VPN Always On:

| Miglioramento chiave | Descrizione |
| ---- | ---- |
| **[Windows Information Protection (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | Integrazione con Windows Information Protection consente di applicazione dei criteri di rete determinare se il traffico è consentito passare attraverso la connessione VPN. Se il profilo utente è attivo e vengono applicati i criteri WIP, VPN Always On viene automaticamente attivata per la connessione. Inoltre, quando si utilizza Windows Information Protection, non è necessario specificare le regole AppTriggerList e TrafficFilterList separatamente nel profilo della VPN (a meno che non vuoi che la configurazione più avanzata) perché i criteri WIP e gli elenchi di applicazione effettivi automaticamente. |
| **[Windows Hello for Business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | VPN Always On in modo nativo supporta Windows Hello for Business (in modalità di autenticazione basata su certificati) per fornire una semplice esperienza single sign-on per entrambi Accedi al computer e la connessione VPN. Di conseguenza, non è necessaria alcuna autenticazione secondaria (credenziali utente) per la connessione VPN, rendendo possibile usare una connessione Always On con Windows Hello for Business l'autenticazione. |
| **[Accesso condizionale di Microsoft Azure](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | Il client VPN Always On può essere integrato con la piattaforma di accesso condizionale di Azure per applicare la conformità dei dispositivi, multi-factor authentication (MFA) o una combinazione dei due. Quando conformi ai criteri di accesso condizionale, Azure Active Directory (Azure AD) emette un certificato di autenticazione breve durato (per impostazione predefinita, 60 minuti) di sicurezza IP (IPsec) che può quindi essere usato per autenticare il gateway VPN. Conformità dei dispositivi Usa i criteri di conformità di System Center Configuration Manager/Intune, che possono includere lo stato attestazione dell'integrità del dispositivo come parte del controllo di conformità di connessione. |
| **Azure MFA** | Quando in combinazione con servizi RADIUS Remote Authentication Dial-In User Service () e l'estensione Server dei criteri di rete (NPS) per Azure MFA, autenticazione VPN possa utilizzare MFA sicuro. |
| **Plug-in VPN di terze parti** | Con la piattaforma UWP (Universal Windows), provider VPN di terze parti possono creare una singola applicazione per l'intervallo completo di Windows 10 dispositivi. La piattaforma UWP offre un livello di API di base garantito su tutti i dispositivi, eliminando così la complessità della e i problemi spesso associati alla scrittura di driver di livello di kernel. Attualmente, plug-in VPN UWP di Windows 10 esistenti per [Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp), [F5 Access](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0), [Check Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj), [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc), [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)e [GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); senza dubbio, gli altri verranno visualizzati in futuro. |
---

## Sicurezza

Miglioramenti principali di sicurezza sono nelle aree seguenti:

| Miglioramento chiave | Descrizione |
| ---- | ---- |
| **Filtri del traffico** | Tramite filtri del traffico, è possibile specificare i criteri lato client che determinano quali traffico consentito nella rete aziendale. In questo modo, gli amministratori possono applicare le restrizioni di app o il traffico sull'interfaccia VPN, limitando l'utilizzo di origini specifiche, porte di destinazione e gli indirizzi IP. Sono disponibili due tipi di regole di filtro:<ul><li>**Regole basate sull'app.** Le regole del firewall basate sull'App si basano su un elenco di applicazioni specificate in modo che solo il traffico proveniente da queste App sono autorizzate a passare attraverso l'interfaccia VPN.</li><li>**Regole basate sul traffico.** Le regole del firewall basate sul traffico sono basate sui requisiti di rete come porte, indirizzi e protocolli. Usa queste regole solo per il traffico che corrisponde a queste condizioni specifiche sono autorizzate a passare attraverso l'interfaccia VPN.<p><p>_**Nota.**_<br>Queste regole si applicano solo per il traffico in uscita dal dispositivo. Uso di traffico filtra il traffico in ingresso blocchi dalla rete aziendale nel client. </li></ul> |
| **Ogni singola App VPN** | Ogni singola App VPN è come disporre di un filtro del traffico basato su app, ma l'esecuzione va oltre per combinare i trigger di applicazione con un filtro del traffico basato su app in modo che la connettività VPN è vincolata a un'applicazione specifica anziché tutte le applicazioni nel client VPN. La funzionalità avvia automaticamente all'avvio dell'app. |
| **Supporto per gli algoritmi di crittografia personalizzati IPsec** | VPN Always On supporta l'uso di RSA e curva ellittico basati su crittografia personalizzato gli algoritmi di crittografia per soddisfare rigorosi per enti pubblici o i criteri di sicurezza dell'organizzazione. |
| **Supporto nativo protocollo EAP (Extensible Authentication)** | VPN Always On in modo nativo supporta il protocollo EAP, che consente di usare un set di Microsoft e i tipi EAP di terze parti come parte del flusso di lavoro autenticazione. Il protocollo EAP fornisce l'autenticazione sicura in base a tipi di autenticazione seguenti:<ul><li>Nome utente e password</li><li>Smart card (fisiche e virtuali)</li><li>Certificati utente</li><li>Windows Hello for Business</li><li>Supporto MFA tramite l'integrazione EAP RADIUS</li></ul>Il fornitore dell'applicazione controlla i metodi di autenticazione plug-in VPN UWP di terze parti, anche se hanno una matrice di opzioni disponibili, inclusi i tipi di credenziali personalizzate e il supporto OTP. |
---

## Connettività VPN

Ecco alcuni miglioramenti principali di connettività VPN Always On:

| Miglioramento chiave | Descrizione |
| ---- | ---- |
| **Sempre attivato** | Sempre attivato è una funzionalità di Windows 10 che consente al profilo VPN per connettersi automaticamente e rimangono connessi in base a trigger, vale a dire, accesso utente in, modifica dello stato di rete o active schermo del dispositivo. Always On anche è integrato nell'esperienza di standby connesso per aumentare la durata della batteria. |
| **Dell'attivazione dell'applicazione** | È possibile configurare i profili VPN in Windows 10 di connettersi automaticamente all'avvio di uno specifico set di applicazioni. È possibile configurare desktop sia nelle applicazioni UWP per attivare una connessione VPN. |
| **Attivazione basata sul nome** | Con VPN Always On, è possibile definire regole in modo che le query di nome di dominio specifico attivano la connessione VPN. Windows 10 supporta ora basati sul nome dell'attivazione per i computer appartenenti al dominio e non di dominio aggiunto (in precedenza, sono supportati solo i computer appartenenti a non di dominio). |
| **Rilevamento della rete attendibile** | VPN Always On include questa funzionalità per garantire che la connettività VPN non venga attivata se un utente è connesso a una rete attendibile all'interno del limite azienda. Questa funzionalità è possibile combinare con uno dei metodi di attivazione in precedenza per offrire un'esperienza utente semplice "Connetti solo all'occorrenza". |
| **[Tunnel dei dispositivi](../vpn-device-tunnel-config.md)** | VPN Always On offre la possibilità di creare un profilo VPN dedicato per dispositivo o un computer. A differenza di _Tunnel utente_, che connette solo dopo un utente accede il dispositivo o un computer, _Al Tunnel dei dispositivi_ consente la connessione VPN stabilire la connessione prima di accesso utente in. Sia al Tunnel dei dispositivi e utenti Tunnel funzionano in modo indipendente con i profili VPN, possono essere connessi allo stesso tempo e possono usare diversi metodi di autenticazione e altre impostazioni di configurazione VPN come appropriato. |
---

## Reti

Di seguito sono indicati alcuni dei miglioramenti di VPN Always On rete:

| Miglioramento chiave | Descrizione |
| ---- | ---- |
| **Supporto per IPv4 e IPv6 dual-stack** | VPN Always On in modo nativo supporta l'uso di IPv4 e IPv6 in un approccio dual-stack. Non include alcuna dipendenza specifico su un protocollo rispetto a altro, che consente di massima IPv4 o IPv6 la compatibilità delle applicazioni in combinazione con il supporto per IPv6 future esigenze di rete.<p>**_Nota._** Prima di iniziare, assicurati di abilitare IPv6 sul server VPN. In caso contrario, non può essere stabilita una connessione e visualizza un messaggio di errore.|
| **Criteri di routing specifiche dell'applicazione** | Oltre a definire globali criteri routing di connessione VPN per la separazione di traffico internet e reti intranet, è possibile aggiungere i criteri di routing per controllare l'uso di split tunneling o forzare configurazioni tunnel su una base per ogni applicazione. Questa opzione offre un controllo più granulare su quali sono consentite le app di interagire con le risorse tramite il tunnel VPN. |
| **Route di esclusione** | VPN Always On supporta la possibilità di specificare route di esclusione che controllano in modo specifico comportamento di routing per definire il tipo di traffico deve attraversare la VPN solo e non passare attraverso l'interfaccia di rete fisica.<p><p>_**Note.**_<br>-Route di esclusione funzionano per il traffico all'interno della stessa subnet del client, ad esempio, LinkLocal.<br>-Route di esclusione funzionano solo una configurazione con Split tunneling. |
---

## Configurazione e compatibilità 

Di seguito sono indicati alcuni dei miglioramenti compatibilità e configurazione di VPN Always On:

| Miglioramento chiave | Descrizione |
| ---- | ---- |
| **Compatibilità di gateway VPN di terze parti** | Il client VPN Always On non richiede l'uso di un gateway VPN basate su Microsoft a funzionare. Grazie al supporto del protocollo IKEv2, il client semplifica l'interoperabilità con i gateway VPN di terze parti che supportano questo tipo di tunneling standard di settore. Puoi anche ottenere interoperabilità con i gateway VPN di terze parti tramite un plug-in VPN UWP in combinazione con un tipo personalizzato tunneling senza compromettere vantaggi e le funzionalità della piattaforma VPN Always On.<p><p>_**Nota.**_<br>Consultare il tuo fornitore di back-end accessorio gateway o di terze parti sulle configurazioni e compatibilità con VPN Always On e Tunnel dei dispositivi mediante IKEv2. |
| **Supporto del protocollo VPN IKEv2 standard di settore** | Il client VPN Always On supporta IKEv2, una delle attuali maggiormente utilizzati protocolli di tunneling standard di settore. Questa compatibilità ingrandisce l'interoperabilità con i gateway VPN di terze parti. |
| **Supporto di piattaforma** | Supporta AAlways su VPN appartenenti al dominio, appartenenti a non di dominio (gruppo di lavoro) o AD – dispositivi aggiunti ad Azure per consentire per entrambe le aziende e portare il dispositivo personale scenari (BYOD). Inoltre, VPN Always On è disponibile in tutte le edizioni di Windows. |
| **Diversi meccanismi di distribuzione e gestione** | È possibile utilizzare molti meccanismi di distribuzione e gestione per gestire le impostazioni di VPN (noto come un _profilo VPN_), incluso Windows PowerShell, System Center Configuration Manager, Intune (o dispositivo mobile di terze parti [] strumento di gestione MDM) e Windows Progettazione configurazione. Queste opzioni di semplificano la configurazione della VPN Always On indipendentemente dal puoi usare gli strumenti di gestione client. |
| **Definizione del profilo VPN standardizzata** | VPN Always On supporta la configurazione tramite un profilo XML standard (ProfileXML), fornendo un modello di formato standard di configurazione che usano la maggior parte dei set di strumenti di distribuzione e la gestione. |
---


## Passaggi successivi

- [Scopri alcune delle funzionalità VPN Always On avanzate](deploy/always-on-vpn-adv-options.md)

- [Altre informazioni sulla tecnologia di VPN Always On](always-on-vpn-technology-overview.md)

- [Iniziare a pianificare la distribuzione di VPN Always On](deploy/always-on-vpn-deploy-deployment.md)



---
