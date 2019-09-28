---
title: Panoramica dell'autenticazione Kerberos
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 646c6309-e865-4be2-b415-44dd125af5c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 33712dc8502035bd9e47e1d2bdd4583eb8347dec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386313"
---
# <a name="kerberos-authentication-overview"></a>Panoramica dell'autenticazione Kerberos

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Kerberos è un protocollo di autenticazione usato per verificare l'identità di un utente o un host. Questo argomento contiene informazioni sull'autenticazione Kerberos in Windows Server 2012 e Windows 8.

## <a name="BKMK_OVER"></a>Descrizione della funzionalità
I sistemi operativi Windows Server implementano il protocollo e le estensioni Kerberos versione 5 per l'autenticazione con chiave pubblica, il trasporto dei dati di autorizzazione e la delega. Il client di autenticazione Kerberos viene implementato come Security Support Provider \(SSP @ no__t-1 ed è possibile accedervi tramite l'interfaccia Security Support Provider \(SSPI @ no__t-3. L'autenticazione iniziale degli utenti è integrata con l'architettura di Winlogon Single Sign @ no__t-0on.

Il Centro distribuzione chiavi Kerberos \(KDC @ no__t-1 è integrato con altri servizi di sicurezza di Windows Server in esecuzione sul controller di dominio. Il KDC usa il database Active Directory Domain Services del dominio come database degli account di sicurezza. Le implementazioni Kerberos predefinite nel dominio o nella foresta richiedono Servizi di dominio Active Directory.

## <a name="kerb_tr_Kerb_Benefits"></a>Applicazioni pratiche
I vantaggi ottenuti tramite Kerberos per l'autenticazione Domain @ no__t-0based sono i seguenti:

-   **Autenticazione delegata.**

    I servizi eseguiti nei sistemi operativi Windows possono rappresentare un computer client quando accedono alle risorse per conto del client. In molti casi, un servizio può completare il proprio lavoro per il client accedendo alle risorse nel computer locale. Quando un computer client esegue l'autenticazione per il servizio, il protocollo NTLM e Kerberos forniscono le informazioni di autorizzazione necessarie al servizio per rappresentare il computer client in locale. Tuttavia, alcune applicazioni distribuite sono progettate in modo da consentire a un servizio front @ no__t-0end di usare l'identità del computer client quando si connette a back @ no__t-1end Services in altri computer. L'autenticazione Kerberos supporta un meccanismo di delega che consente a un servizio di agire per conto del relativo client quando si connette ad altri servizi.

-   **Single Sign-on.**

    L'autenticazione Kerberos in un dominio o in una foresta consente all'utente o al servizio di accedere alle risorse autorizzate dagli amministratori senza più richieste per le credenziali. Dopo l'accesso iniziale al dominio tramite Winlogon, Kerberos gestisce le credenziali in tutta la foresta ogni volta che si prova ad accedere alle risorse.

-   **Interoperabilità.**

    L'implementazione del protocollo Kerberos V5 da parte di Microsoft si basa sulle specifiche standard @ no__t-0track che sono consigliate per Internet Engineering Task Force \(IETF @ no__t-2. Di conseguenza, nei sistemi operativi Windows, il protocollo Kerberos è alla base dell'interoperabilità con altre reti in cui viene usato il protocollo Kerberos per l'autenticazione. Inoltre, Microsoft pubblica la documentazione dei protocolli Windows per l'implementazione del protocollo Kerberos. La documentazione contiene i requisiti tecnici, le limitazioni, le dipendenze e il comportamento del protocollo Windows @ no__t-0specific per l'implementazione del protocollo Kerberos di Microsoft.

-   **Autenticazione più efficace per i server.**

    Prima di Kerberos, veniva usata l'autenticazione NTLM, con cui un server applicazioni deve connettersi a un controller di dominio per autenticare ogni computer client o servizio. Con il protocollo Kerberos, i ticket di sessione rinnovabili sostituiscono l'autenticazione pass @ no__t-0through. Il server non deve accedere a un controller di dominio \(unless è necessario convalidare un certificato di attributo privilegio \(PAC @ no__t-2 @ no__t-3. Il server può invece autenticare il computer client esaminando le credenziali presentate dal client. I computer client possono ottenere le credenziali per un particolare server una sola volta e quindi riusare tali credenziali durante tutta la sessione di accesso alla rete.

-   **Autenticazione reciproca.**

    Con il protocollo Kerberos, una parte a una delle due estremità di una connessione di rete può verificare che la parte all'altra estremità è l'entità che asserisce di essere. NTLM non consente ai client di verificare l'identità di un server o di consentire a un server di verificare l'identità di un altro server. L'autenticazione NTLM è stata progettata per un ambiente di rete in cui si presuppone che i server siano autentici. Il protocollo Kerberos non si basa su tale supposizione.

## <a name="see-also"></a>Vedere anche
[Panoramica di Autenticazione di Windows](../windows-authentication/windows-authentication-overview.md)


