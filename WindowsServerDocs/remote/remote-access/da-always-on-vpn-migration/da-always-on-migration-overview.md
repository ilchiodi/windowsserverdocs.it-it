---
title: Panoramica della migrazione di accesso remoto Always On VPN
description: Always On VPN risolve i gap precedenti tra le VPN Windows e DirectAccess e come eseguire la migrazione da DirectAccess a Always On VPN.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: d3ea6f0e29803b8a709f31811f77678bf03201a8
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822580"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>Panoramica della migrazione di DirectAccess a VPN Always On 

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows 10

&#187;[Passaggio **successivo:** pianificare DirectAccess per Always on la migrazione VPN](da-always-on-migration-planning.md)

Nelle versioni precedenti dell'architettura VPN di Windows, le limitazioni della piattaforma rendevano difficile fornire le funzionalità critiche necessarie per sostituire DirectAccess, ad esempio le connessioni automatiche avviate prima dell'accesso degli utenti. Always On VPN, tuttavia, ha attenuato la maggior parte di tali limitazioni o ha espanso la funzionalità VPN oltre le funzionalità di DirectAccess. Always On VPN risolve i gap precedenti tra le VPN Windows e DirectAccess.

Il processo di migrazione VPN da DirectAccess a Always On è costituito da quattro componenti principali e processi di alto livello:


1.  **Pianificare la migrazione della VPN Always On.** La pianificazione consente di identificare i client di destinazione per la separazione della fase utente, nonché l'infrastruttura e le funzionalità.

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **Distribuire un'infrastruttura VPN affiancata.** Dopo aver determinato le fasi di migrazione e le funzionalità che si desidera includere nella distribuzione, è possibile distribuire l'infrastruttura VPN Always On affiancata all'infrastruttura DirectAccess esistente.  

3.  **Distribuire i certificati e la configurazione ai client.**  Quando l'infrastruttura VPN è pronta, è possibile creare e pubblicare i certificati necessari per il client. Quando i client hanno ricevuto i certificati, distribuire lo script di configurazione VPN_Profile. ps1. In alternativa, è possibile usare Intune per configurare il client VPN. Usare Microsoft endpoint Configuration Manager o Microsoft Intune per monitorare le distribuzioni di configurazione VPN riuscite.

4.  **Rimuovere e rimuovere le autorizzazioni.** Rimuovere correttamente le autorizzazioni per l'ambiente dopo la migrazione di tutti gli utenti da DirectAccess.

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>Scenario di distribuzione di DirectAccess

In questo scenario di distribuzione si usa un semplice scenario di distribuzione di DirectAccess come punto di partenza per la migrazione presente in questa guida. Non è necessario associare questo scenario di distribuzione prima di eseguire la migrazione a Always On VPN. Tuttavia, per molte organizzazioni, questa semplice configurazione è una rappresentazione accurata della distribuzione di DirectAccess corrente. Nella tabella seguente viene fornito un elenco delle funzionalità di base per questa configurazione.

Esistono molti scenari e opzioni di distribuzione di DirectAccess, pertanto è probabile che l'implementazione sia diversa da quella descritta qui. In tal caso, fare riferimento al [mapping delle funzionalità tra DirectAccess e always on VPN](../vpn/vpn-map-da.md) per determinare il mapping del set di funzionalità della VPN always on per le aggiunte correnti, quindi aggiungere tali funzionalità alla configurazione. È anche possibile fare riferimento a [Always on miglioramenti della VPN](../vpn/always-on-vpn/always-on-vpn-enhancements.md) per aggiungere opzioni alla distribuzione di always on VPN.

>[!NOTE] 
>Per i dispositivi non aggiunti a un dominio, è necessario considerare altre considerazioni, ad esempio la registrazione del certificato. Per informazioni dettagliate, vedere [Always on distribuzione VPN per Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md).

### <a name="deployment-scenario-feature-list"></a>Elenco delle funzionalità dello scenario di distribuzione

| Funzionalità DirectAccess | Scenario tipico |
|-----|----|
| Scenario di distribuzione                   | Distribuire DirectAccess completo per l'accesso client e la gestione remota                                               |
| Schede di rete                      | 2                                                                                                              |
| Autenticazione utente                   | Credenziali Active Directory                                                                                   |
| Usa certificati computer             | Sì                                                                                                            |
| Gruppi di sicurezza                       | Sì                                                                                                            |
| Server DirectAccess singolo            | Sì                                                                                                            |
| Topologia di rete                      | NAT (Network Address Translation) dietro un firewall perimetrale con due schede di rete                            |
| Modalità di accesso                           | Da fine a perimetrale                                                                                                    |
| Tunneling                             | Suddividi tunnel                                                                                                   |
| Autenticazione                        | Autenticazione dell'infrastruttura a chiave pubblica (PKI) standard con certificato computer più Kerberos (non KerbProxy) |
| Protocolli                             | IP su HTTPS (IP-HTTPS)                                                                                       |
| Server del percorso di rete (NLS) off-box | Sì                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Scenario di distribuzione VPN Always On

In questo scenario di distribuzione, è possibile concentrarsi sulla migrazione di un ambiente DirectAccess semplice a un semplice ambiente VPN Always On, ovvero la soluzione di sostituzione DirectAccess. La tabella seguente fornisce le funzionalità usate in questa semplice soluzione. Per informazioni più dettagliate su ulteriori miglioramenti apportati al client VPN Always On, vedere [Always on miglioramenti della VPN](../vpn/always-on-vpn/always-on-vpn-enhancements.md).

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>Always On le funzionalità VPN usate nell'ambiente semplice

| Funzionalità VPN | Configurazione dello scenario di distribuzione |
|-----|-----|
| Tipo di connessione | Native Internet Key Exchange versione 2 (IKEv2) |
| Schede di rete   | 2        |
| Autenticazione utente  | Credenziali Active Directory            |
| Usa certificati computer        | Sì                          |
| Routing | Split tunneling |
| Risoluzione dei nomi | Elenco di informazioni sul nome di dominio e suffisso DNS (Domain Name System) |
| Attivazione | Rilevamento della rete always on e attendibile |
| Autenticazione  | PEAP-TLS (Protected Extensible Authentication Protocol-Transport Layer Security) con certificati utente protetti da Trusted Platform Module |

## <a name="next-step"></a>Passaggio successivo

[Pianificare DirectAccess per Always on migrazione VPN](da-always-on-migration-planning.md). L'obiettivo principale della migrazione è consentire agli utenti di mantenere la connettività remota all'ufficio durante tutto il processo.

---