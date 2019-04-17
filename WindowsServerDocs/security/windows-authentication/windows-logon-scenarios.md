---
title: Scenari di accesso di Windows
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66b7c568-67b7-4ac9-a479-a5a3b8a66142
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d6a1d52bee3c2d14ea83ccb794ecea1872868df5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="windows-logon-scenarios"></a>Scenari di accesso di Windows

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento di riferimento per i professionisti IT vengono riepilogati comune accesso a Windows e scenari.

I sistemi operativi Windows richiedono tutti gli utenti di accedere al computer con un account valido per l'accesso locale e risorse di rete. I computer basati su Windows proteggere le risorse implementando il processo di accesso, in cui gli utenti vengono autenticati. After a user is authenticated, authorization and access control technologies implement the second phase of protecting resources: determining if the authenticated user is authorized to access a resource.

Il contenuto di questo argomento si applica a versioni di Windows indicate nel **si applica a** elenco all'inizio di questo argomento.

Inoltre, applicazioni e servizi possono richiedere agli utenti di accedere accesso alle risorse offerte dal servizio o applicazione. Il processo di accesso è simile al processo di accesso, in un account valido e le credenziali corrette sono obbligatori, ma le informazioni di accesso viene archiviate nel database Gestione Account di protezione (SAM) nel computer locale e in Active Directory in cui è applicabile. Account e le credenziali informazioni di accesso viene gestite dal servizio o applicazione e facoltativamente possono essere archiviate in locale nella casella di sicurezza delle credenziali.

Per informazioni su come funziona l'autenticazione, vedere [concetti di autenticazione di Windows](windows-authentication-concepts.md).

In questo argomento vengono descritti gli scenari seguenti:

-   [Accesso interattivo](#BKMK_InteractiveLogon)

-   [Accesso alla rete](#BKMK_NetworkLogon)

-   [Accesso con smart card](#BKMK_SmartCardLogon)

-   [Accesso biometrico](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Accesso interattivo
Il processo di accesso inizia quando un utente immette le credenziali nella finestra di dialogo Immissione credenziali, o quando l'utente inserisce una smart card nel lettore di smart card, o quando l'utente interagisce con un dispositivo biometrico. Gli utenti possono eseguire un accesso interattivo con un account utente locale o un account di dominio per accedere a un computer.

Il diagramma seguente mostra gli elementi di accesso interattivo e un processo di accesso.

![Diagramma che illustra il processo di accesso e gli elementi di accesso interattivo](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Architettura dell'autenticazione di Client Windows**

### <a name="BKMK_LocaDomainLogon"></a>Accesso locali e di dominio
Credenziali di cui dispone l'utente per l'accesso a un dominio contengono tutti gli elementi necessari per l'accesso locale, quali nome account e password o certificato e informazioni di dominio Active Directory. Il processo di conferma l'identificazione dell'utente al database di protezione nel computer locale dell'utente o a un dominio di Active Directory. Questo processo di accesso obbligatorio non può essere disattivato per gli utenti in un dominio.

Gli utenti possono eseguire un accesso interattivo a un computer in uno dei due modi:

-   In locale, quando l'utente ha accesso fisico diretto al computer o quando il computer fa parte di una rete di computer.

    Accesso locale concede a un utente l'autorizzazione per accedere alle risorse di Windows nel computer locale. Accesso locale richiede che l'utente disponga di un account utente in Gestione account di sicurezza (SAM) nel computer locale. Il database SAM protegge e gestisce informazioni sull'utente e gruppo sotto forma di account di sicurezza archiviate nel Registro di sistema computer locale. Il computer può avere accesso alla rete, ma non è obbligatorio. Informazioni di appartenenza a gruppi e account utente locale viene utilizzate per gestire l'accesso alle risorse locali.

    Un utente l'autorizzazione per accedere alle risorse di Windows nel computer locale, oltre a tutte le risorse nei computer in rete, come definito dal token di accesso della credenziale concede l'accesso alla rete. Sia l'accesso locale e un accesso alla rete richiedono che l'utente disponga di un account utente in Gestione account di sicurezza (SAM) nel computer locale. Informazioni di appartenenza a gruppi e account utente locale viene utilizzate per gestire l'accesso alle risorse locali e il token di accesso per l'utente definisce le risorse sono accessibili nei computer in rete.

    Accesso locale e un accesso alla rete non sono sufficienti per concedere l'autorizzazione utente e computer per l'accesso e l'utilizzo delle risorse di dominio.

-   In modalità remota, tramite servizi Terminal o Servizi Desktop remoto (RDS), nel qual caso l'accesso è ulteriormente qualificato come remoto interattivo.

Dopo un accesso interattivo, Windows esegue applicazioni per conto dell'utente e l'utente può interagire con tali applicazioni.

Un utente l'autorizzazione per accedere alle risorse del computer locale o di risorse nei computer in rete concede l'accesso locale. Se il computer è unito a un dominio, la funzionalità di Winlogon tenta di accedere a tale dominio.

Un utente l'autorizzazione di accesso locale e risorse di dominio, consente l'accesso a un dominio. Accesso a un dominio richiede che l'utente disponga di un account utente in Active Directory. Il computer deve disporre di un account di dominio di Active Directory e di essere connesso fisicamente alla rete. Gli utenti devono inoltre disporre dei diritti utente per accedere a un computer locale o un dominio. Informazioni sull'account utente di dominio e informazioni di appartenenza al gruppo vengono utilizzati per gestire l'accesso alle risorse locali e di dominio.

### <a name="BKMK_RemoteLogon"></a>Accesso remoto.
In Windows, l'accesso a un altro computer tramite accesso remoto si basa su Remote Desktop Protocol (RDP). Poiché l'utente deve già avere connesso al computer client prima di tentare una connessione remota, i processi di accesso interattivo sono stati completati correttamente.

RDP gestisce le credenziali immesse dall'utente tramite il Client Desktop remoto. Tali credenziali sono destinate a computer di destinazione e l'utente deve disporre di un account del computer di destinazione. Inoltre, il computer di destinazione deve essere configurato per accettare una connessione remota. Le credenziali del computer di destinazione vengono inviate a tentare di eseguire il processo di autenticazione. Se l'autenticazione ha esito positivo, l'utente è connesso a locale e risorse di rete che sono accessibili tramite le credenziali fornite.

## <a name="BKMK_NetworkLogon"></a>Accesso alla rete
Accesso alla rete può essere utilizzato solo dopo l'autenticazione utente, servizio o computer. Durante l'accesso alla rete, il processo non utilizza le finestre di dialogo immissione di credenziali per raccogliere i dati. Al contrario, precedentemente stabilita credenziali o viene utilizzato un altro metodo per raccogliere le credenziali. Questo processo conferma l'identità dell'utente a qualsiasi servizio di rete che l'utente sta tentando di accedere. Questo processo è in genere invisibile all'utente, a meno che non devono essere forniti credenziali alternative.

Per fornire questo tipo di autenticazione, il sistema di sicurezza include questi meccanismi di autenticazione:

-   Protocollo Kerberos versione 5

-   Certificati di chiave pubblica

-   Secure Sockets Layer/Transport Layer Security (TLS/SSL)

-   Digest

-   NTLM, for compatibility with Microsoft Windows NT 4.0-based systems

Per informazioni sugli elementi e processi, vedere il diagramma di accesso interattivo precedente.

## <a name="BKMK_SmartCardLogon"></a>Accesso con smart card
Le smart card è utilizzabile per accedere solo agli account di dominio, account non locali. Autenticazione con smart card richiede l'utilizzo del protocollo di autenticazione Kerberos. Introdotto in Windows 2000 Server, nei sistemi operativi basati su Windows un'estensione di chiave pubblica per il richiesta di autenticazione iniziale del protocollo è implementato di Kerberos. In contrast to shared secret key cryptography, public key cryptography is asymmetric, that is, two different keys are needed: one to encrypt, another to decrypt. Insieme, le chiavi necessarie per eseguire entrambe le operazioni costituiscono una coppia di chiavi pubblica/privata.

Per avviare una sessione di accesso tipico, un utente deve dimostrare la propria identità, fornendo informazioni note solo all'utente e l'infrastruttura sottostante di protocollo Kerberos. Le informazioni segrete sono una chiave condivisa crittografica derivata dalla password dell'utente. Un segreto condiviso è simmetrico, il che significa che la stessa chiave viene utilizzata per la crittografia e decrittografia.

Il diagramma seguente mostra gli elementi e i processi necessari per l'accesso con smart card.

![Diagramma che mostra gli elementi e i processi necessari per l'accesso con smart card](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Architettura del provider di credenziali Smart Card**

Quando una smart card viene utilizzata invece di una password, una coppia di chiavi pubblica/privata memorizzata su smart card dell'utente viene sostituito con la chiave segreta condivisa, che è derivata dalla password dell'utente. La chiave privata viene archiviata solo sulla smart card. La chiave pubblica può essere resi disponibile a tutti gli utenti con cui vuole il proprietario per lo scambio di informazioni riservate.

Per ulteriori informazioni sul processo di accesso con smart card in Windows, vedere [come smart card funziona l'accesso in Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Accesso biometrico
Un dispositivo viene usato per acquisire e compilare una caratteristica di un elemento, ad esempio un'impronta digitale. Tale rappresentazione digitale viene quindi confrontata con un esempio dell'elemento stesso e quando i due vengono confrontati correttamente, può verificarsi autenticazione. I computer che eseguono uno qualsiasi dei sistemi operativi indicati nel **si applica a** elenco all'inizio di questo argomento può essere configurato per accettare il modulo di accesso. Tuttavia, se l'accesso biometrico è configurato solo per l'accesso locale, l'utente deve presentare le credenziali di dominio quando si accede a un dominio Active Directory.

## <a name="additional-resources"></a>Risorse aggiuntive
Per informazioni su come Windows gestisce le credenziali presentate durante il processo di accesso, vedere [la gestione delle credenziali in autenticazione di Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Panoramica tecnica dell'accesso a Windows e autenticazione](https://technet.microsoft.com/library/dn169029.aspx)


