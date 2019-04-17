---
title: Panoramica dell'autenticazione Kerberos
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9f8878fb959c34663b1e1f3858fa7e0b6e7b6105
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-authentication-overview"></a>Panoramica dell'autenticazione Kerberos

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Kerberos è un protocollo di autenticazione che viene utilizzato per verificare l'identità di un utente o un host. Questo argomento contiene informazioni relative all'autenticazione Kerberos in Windows Server 2012 e Windows 8.

## <a name="BKMK_OVER"></a>Descrizione delle funzionalità
The Windows Server operating systems implement the Kerberos version 5 authentication protocol and extensions for public key authentication, transporting authorization data, and delegation. Il client di autenticazione Kerberos viene implementato come security support provider \(SSP\) e sono accessibili tramite l'interfaccia del Provider supporto sicurezza \(SSPI\). Autenticazione iniziale degli utenti è integrato con l'architettura single Winlogon accesso-on.

The Kerberos Key Distribution Center \(KDC\) is integrated with other Windows Server security services that run on the domain controller. Il KDC utilizza il database di servizi di dominio Active Directory del dominio come database degli account di sicurezza. Servizi di dominio Active Directory è necessario per le implementazioni Kerberos predefinite nel dominio o nella foresta.

## <a name="kerb_tr_Kerb_Benefits"></a>Applicazioni pratiche
I benefici derivanti dall'uso di Kerberos per l'autenticazione basata su domain \ sono:

-   **Autenticazione delegata.**

    I servizi eseguiti nei sistemi operativi Windows possono rappresentare un computer client quando accedono alle risorse per conto del client. In molti casi, un servizio può completare il proprio lavoro per il client accedendo alle risorse del computer locale. Quando un computer client esegue l'autenticazione al servizio, il protocollo NTLM e Kerberos forniscono le informazioni di autorizzazione che un servizio deve rappresentare il computer client in locale. Tuttavia, alcune applicazioni distribuite sono progettate in modo che un servizio front\-end deve usare l'identità del computer client quando si connette ai servizi back\-end in altri computer. L'autenticazione Kerberos supporta un meccanismo di delega che consente a un servizio di agire per conto proprio client durante la connessione ad altri servizi.

-   **Single sign-on.**

    Autenticazione in un dominio o in una foresta con Kerberos consente l'utente o il service l'accesso alle risorse autorizzate dagli amministratori senza più richieste le credenziali. Dopo l'accesso iniziale al dominio tramite Winlogon, Kerberos gestisce le credenziali in tutta la foresta ogni volta che viene tentato l'accesso alle risorse.

-   **Interoperabilità.**

    L'implementazione del protocollo Kerberos V5 da parte di Microsoft si basa sulle specifiche standards\ traccia consigliati per il \(IETF\) Internet Engineering Task Force. Di conseguenza, nei sistemi operativi Windows, il protocollo Kerberos è alla base dell'interoperabilità con altre reti in cui viene usato il protocollo Kerberos per l'autenticazione. Inoltre, Microsoft pubblica documentazione dei protocolli Windows per l'implementazione del protocollo Kerberos. La documentazione contiene i requisiti tecnici, limitazioni, le dipendenze e comportamento del protocollo specifico di Windows per l'implementazione Microsoft del protocollo Kerberos.

-   **Autenticazione più efficace per server.**

    Prima di Kerberos, può essere utilizzata l'autenticazione NTLM, che richiede un server di applicazioni per connettersi a un controller di dominio per autenticare ogni computer client o un servizio. Con il protocollo Kerberos ticket di sessione rinnovabili sostituiscono pass\-tramite l'autenticazione. Il server non è necessario per passare a un controller di dominio \ (a meno che non sia necessario convalidare un \(PAC\)\) certificato attributi privilegi. Il server può invece autenticare il computer client esaminando le credenziali presentate dal client. Computer client possono ottenere le credenziali per un particolare server una sola volta e quindi riusare tali credenziali durante una sessione di accesso di rete.

-   **Autenticazione reciproca.**

    Utilizzando il protocollo Kerberos, un soggetto alle estremità di una connessione di rete può verificare che la parte a altra estremità è l'entità che asserisce di essere. NTLM non consente ai client di verificare l'identità del server o un server di verificare l'identità di un altro server. L'autenticazione NTLM è stato progettato per un ambiente di rete in cui i server si presuppone che siano autentici. Il protocollo Kerberos non deduce tali.

## <a name="see-also"></a>Vedere anche
[Panoramica di autenticazione di Windows](../windows-authentication/windows-authentication-overview.md)


