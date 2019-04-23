---
title: Panoramica della migrazione remota Access VPN Always On
description: VPN Always On di risolvere i gap precedenti tra Windows VPN e DirectAccess e come eseguire la migrazione da DirectAccess a VPN Always On.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 402d8ff72fe869572c9e6129cdf1aa7e755c354a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845982"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>Panoramica della migrazione di DirectAccess a VPN Always On 

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

&#187; [**Next:** Pianificare DirectAccess migrazione da VPN Always On](da-always-on-migration-planning.md)

Nelle versioni precedenti dell'architettura VPN di Windows, delle limitazioni delle piattaforme rendeva difficile fornire la funzionalità critiche necessarie per sostituire DirectAccess, ad esempio le connessioni automatiche avviate prima che gli utenti accedono. VPN Always On, tuttavia, ha mitigato la maggior parte di queste limitazioni o espandere le funzionalità VPN oltre le funzionalità di DirectAccess. VPN Always On di risolvere i gap tra DirectAccess e VPN di Windows precedenti.

DirectAccess – a – sempre sul processo di migrazione VPN è costituita da quattro componenti principali e processi di alto livello:


1.  **Pianificare la migrazione di VPN Always On.** Pianificazione consente di identificare i client di destinazione per la separazione della fase di utente, nonché funzionalità e dell'infrastruttura.

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **Distribuire un'infrastruttura VPN side-by-side.** Dopo aver determinato le fasi di migrazione e le funzionalità da includere nella distribuzione, si distribuisce l'infrastruttura VPN Always On affiancato con l'infrastruttura DirectAccess esistente.  

3.  **Distribuire i certificati e la configurazione per i client.**  Quando l'infrastruttura VPN è pronto, creare e pubblicare i certificati necessari al client. Quando i client hanno ricevuto i certificati, come distribuire lo script di configurazione VPN_Profile.ps1. In alternativa, è possibile usare Intune per configurare il client VPN. Usare Microsoft System Center Configuration Manager o Microsoft Intune per verificare la presenza di distribuzioni completate di configurazione VPN.

4.  **Rimuovere e rimuovere le autorizzazioni.** Rimuovere correttamente le autorizzazioni dell'ambiente dopo che è stata eseguita la migrazione di tutti gli utenti off DirectAccess.

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>Scenario di distribuzione di DirectAccess

In questo scenario di distribuzione, usare un semplice scenario di distribuzione di DirectAccess come punto di partenza per la migrazione di che questa guida vengono illustrati. Non è necessario per la corrispondenza di questo scenario di distribuzione prima della migrazione a VPN Always On, ma per molte organizzazioni, questo semplice programma di installazione è una rappresentazione accurata della loro distribuzione DirectAccess corrente. Nella tabella seguente fornisce un elenco di funzionalità di base per il programma di installazione.

Molti scenari di distribuzione di DirectAccess e le opzioni presenti, pertanto l'implementazione è probabilmente diverso da quello descritto di seguito. In questo caso, consultare [mapping di funzionalità tra DirectAccess e VPN Always On](../vpn/vpn-map-da.md) per determinare la funzionalità di VPN Always On impostare mapping per i componenti aggiuntivi e quindi aggiungere le funzionalità alla propria configurazione. Inoltre, è possibile fare riferimento al [miglioramenti della VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md) aggiungere opzioni per la distribuzione VPN Always On.

>[!NOTE] 
>Per i dispositivi aggiunti a un non di dominio, esistono altre considerazioni, ad esempio la registrazione di certificati. Per informazioni dettagliate, vedere [Always On VPN distribuzione per Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md).

### <a name="deployment-scenario-feature-list"></a>Elenco di funzionalità di uno scenario di distribuzione

| Funzionalità di DirectAccess | Scenario tipico |
|-----|----|
| Scenario di distribuzione                   | Distribuisci DirectAccess completo per l'accesso client e la gestione remota                                               |
| Schede di rete                      | 2                                                                                                              |
| Autenticazione utente                   | Credenziali Active Directory                                                                                   |
| Usa certificati computer             | Yes                                                                                                            |
| Gruppi di sicurezza                       | Yes                                                                                                            |
| Server DirectAccess singolo            | Yes                                                                                                            |
| Topologia di rete                      | Il protocollo NAT (NAT) dietro un firewall periferico con due schede di rete                            |
| Modalità di accesso                           | Terminare al bordo                                                                                                    |
| Tunneling                             | Split tunneling                                                                                                   |
| Autenticazione                        | Autenticazione con certificato computer oltre a Kerberos (non KerbProxy) standard infrastruttura a chiave pubblica (PKI) |
| Protocolli                             | IP tramite HTTPS (IP-HTTPS)                                                                                       |
| Percorso server (NLS) off-finestra di rete | Yes                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Scenario di distribuzione VPN Always On

In questo scenario di distribuzione, concentrarsi sulla migrazione di un ambiente di DirectAccess in un ambiente semplice VPN Always On, che è la soluzione DirectAccess di sostituzione. Nella tabella seguente fornisce le funzionalità usate in questa soluzione semplice. Per altre informazioni sui miglioramenti aggiuntivi per il client VPN Always On, vedere [miglioramenti della VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md).

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>Funzionalità di VPN Always On utilizzate nell'ambiente di semplice

| Funzionalità VPN | Configurazione di uno scenario di distribuzione |
|-----|-----|
| Tipo di connessione | Native Internet Key Exchange version 2 (IKEv2) |
| Schede di rete   | 2        |
| Autenticazione utente  | Credenziali Active Directory            |
| Usa certificati computer        | Yes                          |
| Routing | Lo Split Tunneling |
| Risoluzione dei nomi | Elenco di informazioni sul nome di dominio e il suffisso Domain Name System (DNS) |
| Attivazione | Rilevamento di rete Always on e attendibile |
| Autenticazione  | Protected Extensible Authentication Protocol-Transport Layer Security (PEAP-TLS) con i certificati utente Trusted Platform Module – protetti |

## <a name="next-step"></a>Passaggio successivo

[Pianificare la migrazione da VPN Always On DirectAccess](da-always-on-migration-planning.md). L'obiettivo principale della migrazione è agli utenti di mantenere la connettività remota all'ufficio nel corso del processo.

---