---
title: Miglioramenti di VPN Always On
description: VPN Always On offre numerosi vantaggi rispetto alle soluzioni VPN di Windows del passato. I principali miglioramenti sono in integrazione, sicurezza, connettività, controllo di rete e compatibilità VPN Always On si allineano con visione cloud e incentrato su dispositivi mobili di Microsoft.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836522"
---
# <a name="always-on-vpn-enhancements"></a>Miglioramenti di VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

&#171;[ **Precedente:** Informazioni sulle funzionalità di VPN Always On](../vpn-map-da.md)<br>
&#187; [**Next:** Apprendere le tecnologie VPN Always On](always-on-vpn-technology-overview.md)

VPN Always On offre numerosi vantaggi rispetto alle soluzioni VPN di Windows del passato. I miglioramenti principali seguenti allineano VPN Always On con visione cloud e incentrato su dispositivi mobili di Microsoft:

- **Integrazione di piattaforma:** VPN Always On ha integrazione migliorata con il sistema operativo Windows e le soluzioni di terze parti per offrire una piattaforma solida per innumerevoli scenari di connessione avanzati.

- **Sicurezza:** VPN Always On offre funzionalità di sicurezza nuove e avanzate per limitare il tipo di traffico, quali applicazioni possono usare la connessione VPN, e quali metodi di autenticazione è possibile usare per avviare la connessione. Quando la connessione è attiva la maggior parte dei casi, è particolarmente importante proteggere la connessione. Per altre informazioni, vedere [opzioni di autenticazione VPN](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication).

- **Connettività VPN:** VPN Always On, con o senza dispositivi Tunnel fornisce funzionalità di attivazione automatica. Prima di VPN Always On, la possibilità di attivare una connessione automatica tramite l'autenticazione del dispositivo o utente non era possibile.  

- **Controllo di rete:** VPN Always On consente agli amministratori di specificare criteri di routing a un livello più granulare, persino fino alle applicazioni individuali, ovvero si adatta perfettamente anche per le app line-of-business (LOB) che richiedono l'accesso remoto speciale.  VPN Always On è completamente compatibile anche con entrambi protocollo Internet versione 4 (IPv4) e la versione 6 (IPv6). A differenza di DirectAccess, non è presente alcuna dipendenza specifica IPv6.

  >[!Note]
  >Prima di iniziare, assicurarsi di abilitare IPv6 del server VPN. In caso contrario, non è possibile stabilire una connessione e viene visualizzato un messaggio di errore.
  
- **Configurazione e compatibilità:** VPN Always On possono essere distribuiti e gestiti diversi modi, che offre diversi vantaggi rispetto all'altro software VPN client VPN Always On.

## <a name="platform-integration"></a>Integrazione di piattaforma

Microsoft ha introdotte o migliorate le funzionalità di integrazione seguenti nella VPN Always On:

| Utilizzo chiave | Descrizione |
| ---- | ---- |
| **[Windows Information Protection (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | Integrazione con WIP consente imposizione dei criteri di rete determinare se il traffico è consentito passare tramite la connessione VPN. Se il profilo utente è attivo e vengono applicati i criteri WIP, VPN Always On viene attivata automaticamente per la connessione. Inoltre, quando si usa WIP, non è necessario specificare separatamente AppTriggerList e TrafficFilterList regole nel profilo VPN (a meno che non si desidera che la configurazione più avanzata) poiché gli elenchi di applicazione e i criteri WIP diventano automaticamente effettive. |
| **[Windows Hello for Business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | VPN Always On in modalità nativa supporta Windows Hello for Business (in modalità di autenticazione basata su certificato) per garantire una facile esperienza single sign-on per entrambi accesso al computer e la connessione alla VPN. Pertanto, non è necessaria alcuna autenticazione secondaria (credenziali utente) per la connessione VPN, rendendo possibile usare una connessione sempre attiva con Windows Hello per l'autenticazione di Business. |
| **[Accesso condizionale di Microsoft Azure](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | Il client VPN Always On può essere integrato con la piattaforma di accesso condizionale di Azure per applicare multi-factor authentication (MFA), la conformità dei dispositivi o una combinazione dei due. Quando conformi ai criteri di accesso condizionale, Azure Active Directory (Azure AD) rilascia un certificato di autenticazione IPsec (IP Security) temporanea (per impostazione predefinita, 60 minuti) che può quindi essere usato per autenticare al gateway VPN. Conformità del dispositivo usa i criteri di conformità di System Center Configuration Manager o Intune, che possono includere lo stato di attestazione dell'integrità di dispositivo come parte del controllo di conformità di connessione. |
| **Azure MFA** | Se combinato con servizi RADIUS Remote Authentication Dial-In User Service () e l'estensione Server dei criteri di rete (NPS) per Azure MFA, l'autenticazione VPN può usare MFA avanzata. |
| **Plug-in VPN di terze parti** | Con la Universal Windows Platform (UWP), i provider VPN di terze parti possono creare una singola applicazione per l'intervallo completo di Windows 10 dispositivi. La piattaforma UWP offre un livello di API core garantita tra dispositivi, eliminando la complessità di e i problemi spesso associati alla scrittura di driver a livello di kernel. Attualmente, i plug-in VPN di Windows 10 UWP esistenti per [Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp), [accesso F5](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0), [Check Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj), [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc), [ SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz), e [GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); senza dubbio, gli altri verranno visualizzati in futuro. |
---

## <a name="security"></a>Sicurezza

I miglioramenti principali in sicurezza sono nelle aree seguenti:

| Utilizzo chiave | Descrizione |
| ---- | ---- |
| **Filtri traffico** | Tramite i filtri traffico, è possibile specificare i criteri lato client che determinano quale traffico è consentito nella rete aziendale. In questo modo, gli amministratori possono applicare le restrizioni di app o il traffico nell'interfaccia VPN, limitando l'uso di origini specifiche, le porte di destinazione e gli indirizzi IP. Sono disponibili due tipi di regole di filtro:<ul><li>**Regole basate su app.** Le regole del firewall basato su app si basano su un elenco di applicazioni specificate in modo che solo il traffico proveniente da queste App sono la possibilità di passare attraverso l'interfaccia VPN.</li><li>**Regole in base al traffico.** Le regole del firewall in base al traffico si basano sui requisiti di rete, ad esempio i protocolli, indirizzi e porte. Queste regole solo per il traffico che soddisfa queste condizioni specifiche possono passare tramite l'interfaccia VPN da usare.<p><p>_**Si noti.**_<br>Queste regole si applicano solo al traffico in uscita dal dispositivo. Utilizzo del traffico consente di filtrare il traffico in ingresso blocchi dalla rete aziendale al client. </li></ul> |
| **Per-App VPN** | Profilo VPN per App è come avere un filtro di traffico basati su app, ma diventa più lontano per combinare i trigger di applicazione con un filtro di traffico basati su app in modo che la connettività VPN è limitata a un'applicazione specifica invece di tutte le applicazioni nel client VPN. La funzionalità avvia automaticamente all'avvio dell'app. |
| **Supporto per algoritmi di crittografia IPsec personalizzati** | VPN Always On supporta l'uso di RSA e a curva ellittica basati su crittografia personalizzata gli algoritmi di crittografia per soddisfare i rigorosi per enti pubblici o i criteri di sicurezza dell'organizzazione. |
| **Supporto nativo Extensible Authentication Protocol (EAP)** | VPN Always On supporta in modo nativo EAP, che consente di usare un insieme eterogeneo di Microsoft e i tipi EAP di terze parti come parte del flusso di lavoro autenticazione. EAP fornisce l'autenticazione sicura in base ai tipi di autenticazione seguenti:<ul><li>Nome utente e password</li><li>Smart card (fisiche e virtuali)</li><li>Ottenere certificati utente</li><li>Windows Hello for Business</li><li>Supporto MFA tramite l'integrazione RADIUS EAP</li></ul>Il fornitore dell'applicazione consente di controllare i metodi di autenticazione plug-in UWP VPN di terze parti, anche se è disponibile una gamma di opzioni disponibili, inclusi i tipi di credenziali personalizzate e supporto OTP. |
---

## <a name="vpn-connectivity"></a>Connettività VPN

Di seguito sono i principali miglioramenti nella connettività VPN Always On:

| Utilizzo chiave | Descrizione |
| ---- | ---- |
| **Always On** | Always On è una funzionalità di Windows 10 che permette il profilo VPN attivo per connettersi automaticamente e mantenere la connessione basata su trigger, vale a dire, accesso dell'utente, modifica dello stato di rete o schermo del dispositivo attivo. Always On è anche integrato nell'esperienza di standby connesso per ottimizzare la durata della batteria. |
| **Attivazione dell'applicazione** | È possibile configurare i profili VPN in Windows 10 per connettersi automaticamente all'avvio di un set di applicazioni specificato. È possibile configurare sia desktop e applicazioni della piattaforma UWP per attivare una connessione VPN. |
| **Attivazione basata su nome** | Con VPN Always On, è possibile definire regole in modo che le query di nomi di dominio specifico attivano la connessione VPN. Windows 10 supporta ora in base al nome l'attivazione per le macchine appartenenti a un dominio e aggiunti a un non di dominio (in precedenza, erano supportate solo le macchine appartenenti a un non di dominio). |
| **Rilevamento della rete attendibile** | VPN Always On include questa funzionalità per garantire che la connettività VPN non viene generata se un utente è connesso a una rete attendibile all'interno dell'azienda. È possibile combinare questa funzionalità con uno dei metodi di attivazione indicati in precedenza per fornire un'esperienza utente "connettersi solo quando necessario". |
| **[Dispositivo Tunnel](../vpn-device-tunnel-config.md)** | VPN Always On offre la possibilità di creare un profilo VPN dedicato per computer o dispositivo. A differenza _utente Tunnel_, che si connette solo dopo un utente accede al dispositivo o computer _dispositivo Tunnel_ consente la connessione VPN stabilire la connessione prima dell'accesso dell'utente. Dispositivo Tunnel sia utente Tunnel funzionano in modo indipendente con i propri profili VPN, possono essere connesse allo stesso tempo e possono usare diversi metodi di autenticazione e altre impostazioni di configurazione VPN come appropriato. |
---

## <a name="networking"></a>Rete

Di seguito sono riportati alcuni miglioramenti a livello di rete VPN Always On:

| Utilizzo chiave | Descrizione |
| ---- | ---- |
| **Supporto di dual stack per IPv4 e IPv6** | VPN Always On supporta in modo nativo l'utilizzo di IPv4 e IPv6 in un approccio dual stack. Non ha alcuna dipendenza specifica su un protocollo rispetto a altro, che consente la massima compatibilità delle applicazioni IPv4 e IPv6 combinato con il supporto per IPv6 future esigenze di rete.<p>**_Si noti._** Prima di iniziare, assicurarsi di abilitare IPv6 del server VPN. In caso contrario, non è possibile stabilire una connessione e viene visualizzato un messaggio di errore.|
| **Criteri di routing specifico dell'applicazione** | Oltre a definire globali criteri VPN di routing di connessione per la separazione del traffico internet e intranet, è possibile aggiungere criteri di routing per controllare l'utilizzo di split tunneling o forzare le configurazioni di tunnel su una base per ogni applicazione. Questa opzione offre un controllo più granulare su quali App sono autorizzate a interagire con le risorse attraverso il tunnel VPN. |
| **Route di esclusione** | VPN Always On supporta la possibilità di specificare le route di esclusione che controllano il comportamento di routing per definire il tipo di traffico deve attraversare la connessione VPN solo e passi attraverso l'interfaccia di rete fisica in modo specifico.<p><p>_**Note.**_<br>-Esclusione attualmente il funzionamento delle route per il traffico all'interno della stessa subnet del client, ad esempio, LinkLocal.<br>-Le route esclusione funzionano solo in una configurazione Split tunneling. |
---

## <a name="configuration-and-compatibility"></a>Configurazione e compatibilità 

Di seguito sono riportati alcuni dei miglioramenti configurazione e compatibilità di VPN Always On:

| Utilizzo chiave | Descrizione |
| ---- | ---- |
| **Compatibilità di gateway VPN di terze parti** | Il client VPN Always On non richiede l'uso di un gateway VPN basato su Microsoft per il funzionamento. Grazie al supporto del protocollo IKEv2, il client facilita l'interoperabilità con i gateway VPN di terze parti che supportano questo tipo di tunneling standard del settore. È anche possibile ottenere l'interoperabilità con i gateway VPN di terze parti usando un plug-in VPN UWP combinati con un tipo di tunneling personalizzato senza sacrificare vantaggi e funzionalità della piattaforma VPN Always On.<p><p>_**Si noti.**_<br>Rivolgersi al fornitore dell'appliance di back-end gateway o di terze parti in configurazioni e garantire la compatibilità con VPN Always On e il Tunnel di dispositivo utilizzando IKEv2. |
| **Supporto del protocollo VPN IKEv2 standard del settore** | Il client VPN Always On supporta IKEv2, uno degli attuali più ampiamente utilizzati i protocolli di tunneling standard del settore. Questa compatibilità massimizza l'interoperabilità con i gateway VPN di terze parti. |
| **Supporto della piattaforma** | Supporta VPN On AAlways appartenenti a un dominio, aggiunto non di dominio (gruppo di lavoro) o Azure AD: i dispositivi aggiunti a consentono entrambi le aziende e portare il proprio dispositivo) negli scenari BYOD. Inoltre, VPN Always On è disponibile in tutte le edizioni di Windows. |
| **Diversi meccanismi di distribuzione e gestione** | È possibile usare molti meccanismi di distribuzione e gestione per gestire le impostazioni VPN (chiamato un' _profilo VPN_), tra cui Windows PowerShell, System Center Configuration Manager (Intune o lo strumento di gestione [MDM] dei dispositivi mobili di terze parti) e Progettazione configurazione di Windows. Queste opzioni per semplificare la configurazione della VPN Always On indipendentemente dal fatto gli strumenti di gestione di client che usi. |
| **Definizione del profilo VPN standardizzato** | VPN Always On supporta la configurazione usa un profilo XML standard (ProfileXML), che fornisce un modello di formato standard di configurazione che usano la maggior parte di gestione e i set di strumenti di distribuzione. |
---


## <a name="next-steps"></a>Passaggi successivi

- [Informazioni su alcune delle funzionalità avanzate VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Altre informazioni relative alle tecnologie VPN Always On](always-on-vpn-technology-overview.md)

- [Iniziare a pianificare la distribuzione VPN Always On](deploy/always-on-vpn-deploy-deployment.md)



---
