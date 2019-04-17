---
title: Funzionalità avanzate di VPN Always On
description: Oltre lo scenario di distribuzione fornito in questa distribuzione, è possibile aggiungere altre funzionalità avanzate di VPN per migliorare la sicurezza e la disponibilità della connessione VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: a544ac3c1a121874170a2fc78a34bd401b8bebe1
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067430"
---
# Funzionalità avanzate di VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** ulteriori informazioni sulla tecnologia di VPN Always On](../always-on-vpn-technology-overview.md)<br>
& #187; [ **Successivo:** iniziare a pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-planning.md)

Oltre a scenari di distribuzione forniti, puoi aggiungere altre funzionalità avanzate di VPN per migliorare la sicurezza e la disponibilità della connessione VPN. Ad esempio, tali componenti possono consentono di garantire che il connessione client è integro prima di consentire una connessione.


## Disponibilità elevata

Di seguito sono opzioni aggiuntive per una disponibilità elevata.


|Opzione  |Descrizione  |
|---------|---------|
|Testiamo server e il bilanciamento del carico     |In ambienti che richiedono elevata disponibilità o supporto un numero elevato di richieste, puoi aumentare le prestazioni e resilienza di accesso remoto con bilanciamento del carico tra più server che eseguono Server dei criteri di rete (NPS) e il telecomando di abilitazione Cluster di server di accesso.<p>Documenti correlati:<ul><li>[Bilanciamento del carico Server Proxy dei criteri di rete](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Distribuire Accesso remoto in un cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Resilienza del sito geografica     |Per la georilevazione basate su IP, è possibile utilizzare Gestione traffico globale con il servizio DNS in Windows Server 2016. Per più affidabile geografica il bilanciamento del carico, è possibile utilizzare soluzioni globale Server del bilanciamento del carico, ad esempio Gestione traffico di Microsoft Azure.<p>Documenti correlati:<ul><li>[Panoramica di gestione del traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Gestione traffico di Microsoft Azure](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

---

## Autenticazione avanzata

Di seguito sono opzioni aggiuntive per l'autenticazione.


|Opzione  |Descrizione  |
|---------|---------|
|Windows Hello for Business     |In Windows10, su PC e dispositivi mobili Windows Hello for Business sostituisce l'autenticazione tramite password con l'autenticazione avanzata a due fattori. Questa autenticazione è costituita da un nuovo tipo di credenziali utente associate a un dispositivo e utilizza una tecnologia biometrica o numero di PIN (Personal Identification).<p>Il client VPN di Windows 10 è compatibile con Windows Hello for Business. Dopo che l'utente accede con un movimento, la connessione VPN Usa Windows Hello per il certificato aziendale per l'autenticazione basata su certificati.<p>Documenti correlati:<ul><li>[Windows Hello for Business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Case Study tecnico:, [consentendo l'accesso remoto con Windows Hello for Business in Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure multi-factor Authentication (MFA)     |Azure MFA ha cloud e le versioni che è possibile integrare il meccanismo di autenticazione VPN di Windows in locale.<p>Per altre informazioni sul funzionamento di questo meccanismo, vedi [raggio integrare l'autenticazione con il Server Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

---

## Funzionalità avanzate VPN

Di seguito sono opzioni aggiuntive per le funzionalità avanzate.


|Opzione  |Descrizione  |
|---------|---------|
|Il filtro del traffico     |Se hai bisogno di imporre i client VPN quali applicazioni possono accedere, è possibile attivare filtri del traffico VPN.<p>Per altre informazioni, vedere [la funzionalità di sicurezza VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN attivate App     |È possibile configurare i profili VPN di connettersi automaticamente all'avvio di determinate applicazioni o i tipi di applicazioni.<p>Per ulteriori informazioni su questa e altre opzioni di attivazione, vedere [Opzioni del profilo ad attivazione automatica VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Accesso condizionale VPN   |Conformità di dispositivo e accesso condizionale possono richiedere i dispositivi gestiti per soddisfare gli standard prima che possa connettersi alla VPN. Una delle funzionalità avanzate per l'accesso condizionale VPN consente di limitare le connessioni VPN solo a quelli in cui il certificato di autenticazione client contiene 'Degli AAD l'accesso condizionale OID del ' 1.3.6.1.4.1.311.87'.<p>Per limitare le connessioni VPN, è necessario:<ol><li>Nel server dei criteri di rete, Apri lo snap-in **Server dei criteri di rete** .</li><li>Espandere **criteri** > **i criteri di rete**.</li><li>I criteri di rete **le connessioni di rete privata virtuale (VPN)** destro e scegli **Properties**.</li><li>Seleziona la scheda **Impostazioni** .</li><li>Seleziona **Specifico del fornitore** e fai clic su **Aggiungi**.</li><li>Seleziona l'opzione **Consentite-certificato-OID** e quindi fai clic su **Aggiungi**.</li><li>Incollare l'OID di accesso condizionale AAD dell' **1.3.6.1.4.1.311.87** come valore di attributo e quindi fai clic su **OK** due volte.</li><li>Fai clic su **Chiudi** e quindi **applicare**.<p>Ora quando i client VPN tentano di connettersi usando un certificato diverso dal certificato di breve durata cloud, la connessione avrà esito negativo.</li></ol>Per altre informazioni sull'accesso condizionale, vedi [VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |

---


## Maggiore protezione

**Attestazione della chiave Trusted Platform Module (TPM)**<p>
Un certificato dell'utente con una chiave con attestazione TPM offre una garanzia di sicurezza superiore, il backup non esportabilità, anti-hammering e isolamento delle chiavi fornite dal TPM.

Per ulteriori informazioni sull'attestazione della chiave TPM in Windows 10, vedi [l'Attestazione della chiave TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## Passaggio successivo
[Iniziare a pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-planning.md): prima di installare il ruolo del server Accesso remoto nel computer di cui si intende sull'utilizzo come server VPN, eseguire le attività seguenti. Dopo la pianificazione corretta, puoi distribuire VPN Always On e facoltativamente configurare l'accesso condizionale per la connettività VPN con Azure AD.  

---

## Argomenti correlati
- [Il bilanciamento del carico di Server Proxy dei criteri di rete](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): i client RADIUS Remote Authentication Dial-In User Service (), che sono i server di accesso di rete come server di rete privata virtuale (VPN) e punti di accesso wireless, creare richieste di connessione e inviarle al raggio Server, ad esempio dei criteri di rete. In alcuni casi, un server dei criteri di rete potresti ricevere un numero eccessivo di richieste di connessione in una sola volta, come conseguenza della riduzione delle prestazioni o un overload.

- [Panoramica di gestione del traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): in questo argomento offre una panoramica di Azure il traffico Manager, che consente di controllare la distribuzione del traffico di utenti per gli endpoint del servizio. Gestione traffico Usa il sistema DNS (Domain Name) per indirizzare le richieste del client per endpoint più appropriato in base a un metodo di routing del traffico e l'integrità degli endpoint. 

- [Windows Hello for Business](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): in questo argomento fornisce i prerequisiti, ad esempio distribuzioni solo cloud e ibride.  Questo argomento elenca anche domande frequenti su Windows Hello for Business.

- [Case study tecnico: abilitazione di accesso remoto con Windows Hello for Business in Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): In questo case study tecnico scopriremo come Microsoft implementa l'accesso remoto con Windows Hello for Business.  Windows Hello for Business è una chiave privata/pubblica o un approccio di autenticazione basata su certificati per le aziende e i consumer che va oltre le password. Questa forma di autenticazione si basa sulle credenziali coppia di chiavi che possono sostituire le password e sono resistente alle violazioni, furti e phishing. 

- [Raggio integrare l'autenticazione con il Server Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): in questo argomento illustra tramite l'aggiunta e la configurazione di autenticazione client RADIUS con Server Azure multi-Factor Authentication. RAGGIO è un protocollo standard per accettare le richieste di autenticazione e per elaborare le richieste. Il Server Azure multi-Factor Authentication può fungere da server RADIUS. 

- [Funzionalità di sicurezza di rete VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): in questo argomento fornisce linee guida per la sicurezza VPN per LockDown VPN, integrazione di Windows Information Protection (WIP) con VPN, e applicazione di filtri di traffico. 

- [Opzioni del profilo ad attivazione automatica VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): in questo argomento offre opzioni del profilo ad attivazione automatica VPN, come trigger per app, trigger basati sul nome e Always On.

- [VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): in questo argomento fornisce una panoramica delle basata sul cloud di piattaforma di accesso condizionale per fornire un'opzione di conformità dei dispositivi per i client remoti. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD). 

- [Attestazione della chiave TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): in questo argomento fornisce una panoramica di Trusted Platform Module (TPM) e passaggi per distribuire i TPM la chiave dell'attestazione. Puoi anche trovare passaggi per risolvere i problemi e informazioni sulla risoluzione dei problemi. 

---