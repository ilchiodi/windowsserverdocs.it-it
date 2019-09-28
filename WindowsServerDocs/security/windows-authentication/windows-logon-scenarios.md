---
title: Scenari di accesso di Windows
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 07bfb538e1b43fc0c734b3c59b906c027ef985c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403299"
---
# <a name="windows-logon-scenarios"></a>Scenari di accesso di Windows

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento di riferimento per i professionisti IT riepiloga gli scenari comuni di accesso a Windows e di accesso.

I sistemi operativi Windows richiedono che tutti gli utenti accedano al computer con un account valido per accedere alle risorse locali e di rete. I computer basati su Windows proteggono le risorse implementando il processo di accesso, in cui gli utenti vengono autenticati. Dopo l'autenticazione di un utente, le tecnologie di autorizzazione e controllo di accesso implementano la seconda fase della protezione delle risorse: determinare se l'utente autenticato è autorizzato ad accedere a una risorsa.

Il contenuto di questo argomento si applica alle versioni di Windows designate nell'elenco **si applica a** all'inizio di questo argomento.

Inoltre, le applicazioni e i servizi possono richiedere agli utenti di accedere alle risorse offerte dall'applicazione o dal servizio. Il processo di accesso è simile al processo di accesso, in quanto sono necessari un account valido e le credenziali corrette, ma le informazioni di accesso sono archiviate nel database di gestione account di sicurezza (SAM) nel computer locale e in Active Directory ove applicabile. L'account di accesso e le informazioni sulle credenziali vengono gestite dall'applicazione o dal servizio e, facoltativamente, possono essere archiviate localmente nella casella di sicurezza delle credenziali.

Per informazioni sul funzionamento dell'autenticazione, vedere [concetti relativi all'autenticazione di Windows](windows-authentication-concepts.md).

In questo argomento vengono descritti gli scenari seguenti:

-   [Accesso interattivo](#BKMK_InteractiveLogon)

-   [Accesso alla rete](#BKMK_NetworkLogon)

-   [Accesso con smart card](#BKMK_SmartCardLogon)

-   [Accesso biometrico](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Accesso interattivo
Il processo di accesso inizia quando un utente immette le credenziali nella finestra di dialogo voce credenziali oppure quando l'utente inserisce una smart card nel lettore di smart card o quando l'utente interagisce con un dispositivo biometrico. Gli utenti possono eseguire un accesso interattivo utilizzando un account utente locale o un account di dominio per accedere a un computer.

Il diagramma seguente Mostra gli elementi di accesso interattivo e il processo di accesso.

![Diagramma che Mostra gli elementi di accesso interattivo e il processo di accesso](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Architettura di autenticazione client Windows**

### <a name="BKMK_LocaDomainLogon"></a>Accesso locale e di dominio
Le credenziali presentate dall'utente per l'accesso al dominio contengono tutti gli elementi necessari per un accesso locale, ad esempio il nome dell'account e la password o il certificato, nonché Active Directory informazioni sul dominio. Il processo conferma l'identificazione dell'utente nel database di sicurezza nel computer locale dell'utente o in un dominio Active Directory. Questo processo di accesso obbligatorio non può essere disattivato per gli utenti di un dominio.

Gli utenti possono eseguire un accesso interattivo a un computer in uno dei due modi seguenti:

-   Localmente, quando l'utente ha accesso fisico diretto al computer oppure quando il computer fa parte di una rete di computer.

    Un accesso locale concede a un utente l'autorizzazione per accedere alle risorse di Windows sul computer locale. Per un accesso locale è necessario che l'utente disponga di un account utente in Gestione account di protezione (SAM) nel computer locale. Il SAM protegge e gestisce le informazioni relative a utenti e gruppi sotto forma di account di sicurezza archiviati nel registro di sistema del computer locale. Il computer può disporre di accesso alla rete, ma non è obbligatorio. Per gestire l'accesso alle risorse locali vengono utilizzate le informazioni relative all'account utente locale e all'appartenenza a gruppi.

    Un accesso di rete concede a un utente l'autorizzazione per accedere alle risorse di Windows sul computer locale, oltre a tutte le risorse nei computer in rete in base a quanto definito dal token di accesso della credenziale. Per l'accesso locale e per l'accesso alla rete è necessario che l'utente disponga di un account utente in Gestione account di protezione (SAM) nel computer locale. Le informazioni relative all'appartenenza a gruppi e agli account utente locali vengono usate per gestire l'accesso alle risorse locali e il token di accesso per l'utente definisce le risorse a cui è possibile accedere nei computer in rete.

    Un accesso locale e un accesso alla rete non sono sufficienti per concedere all'utente e al computer l'autorizzazione per l'accesso e l'utilizzo delle risorse di dominio.

-   In modalità remota, tramite Servizi terminal o Servizi Desktop remoto (RDS), nel qual caso l'accesso viene qualificato ulteriormente come Remote Interactive.

Dopo un accesso interattivo, Windows esegue le applicazioni per conto dell'utente e l'utente può interagire con tali applicazioni.

Un accesso locale concede a un utente l'autorizzazione per accedere alle risorse sul computer locale o sulle risorse nei computer in rete. Se il computer fa parte di un dominio, la funzionalità Winlogon tenterà di accedere a tale dominio.

Un accesso al dominio concede a un utente l'autorizzazione per accedere alle risorse locali e di dominio. Per l'accesso al dominio è necessario che l'utente disponga di un account utente in Active Directory. Il computer deve disporre di un account nel dominio Active Directory e essere fisicamente connesso alla rete. Gli utenti devono disporre anche dei diritti utente per accedere a un computer locale o a un dominio. Informazioni sull'account utente di dominio e le informazioni sull'appartenenza a gruppi vengono utilizzate per gestire l'accesso a risorse di dominio e locali.

### <a name="BKMK_RemoteLogon"></a>Accesso remoto
In Windows, l'accesso a un altro computer tramite accesso remoto si basa sul Remote Desktop Protocol (RDP). Poiché l'utente deve avere già eseguito l'accesso al computer client prima di tentare una connessione remota, i processi di accesso interattivo sono stati completati correttamente.

RDP gestisce le credenziali immessi dall'utente tramite il client Desktop remoto. Queste credenziali sono destinate al computer di destinazione e l'utente deve disporre di un account nel computer di destinazione. Inoltre, il computer di destinazione deve essere configurato per accettare una connessione remota. Le credenziali del computer di destinazione vengono inviate per tentare di eseguire il processo di autenticazione. Se l'autenticazione ha esito positivo, l'utente è connesso a risorse locali e di rete accessibili usando le credenziali fornite.

## <a name="BKMK_NetworkLogon"></a>Accesso alla rete
Un accesso alla rete può essere utilizzato solo dopo che è stata eseguita l'autenticazione di utenti, servizi o computer. Durante l'accesso alla rete, il processo non usa le finestre di dialogo di immissione delle credenziali per raccogliere i dati. Vengono invece utilizzate le credenziali stabilite in precedenza o un altro metodo per raccogliere le credenziali. Questo processo conferma l'identità dell'utente per qualsiasi servizio di rete a cui l'utente sta tentando di accedere. Questo processo è in genere invisibile all'utente, a meno che non sia necessario fornire credenziali alternative.

Per fornire questo tipo di autenticazione, il sistema di sicurezza include i meccanismi di autenticazione seguenti:

-   Protocollo Kerberos versione 5

-   Certificati di chiave pubblica

-   Secure Sockets Layer/Transport Layer Security (SSL/TLS)

-   Digest

-   NTLM, per la compatibilità con i sistemi basati su Microsoft Windows NT 4,0

Per informazioni sugli elementi e sui processi, vedere il diagramma di accesso interattivo riportato sopra.

## <a name="BKMK_SmartCardLogon"></a>Accesso con smart card
Le smart card possono essere usate per accedere solo agli account di dominio, non agli account locali. Per l'autenticazione con smart card è necessario usare il protocollo di autenticazione Kerberos. Introdotta in Windows 2000 Server, in sistemi operativi basati su Windows un'estensione di chiave pubblica per la richiesta di autenticazione iniziale del protocollo Kerberos viene implementata. Diversamente dalla crittografia a chiave privata condivisa, la crittografia a chiave pubblica è asimmetrica, ovvero sono necessarie due chiavi diverse: una per la crittografia, un'altra per la decrittografia. Insieme, le chiavi necessarie per eseguire entrambe le operazioni costituiscono una coppia di chiavi pubblica/privata.

Per avviare una tipica sessione di accesso, un utente deve dimostrare la propria identità fornendo informazioni note solo all'utente e all'infrastruttura del protocollo Kerberos sottostante. Le informazioni segrete sono una chiave condivisa crittografica derivata dalla password dell'utente. Una chiave privata condivisa è simmetrica, il che significa che la stessa chiave viene usata sia per la crittografia che per la decrittografia.

Il diagramma seguente Mostra gli elementi e i processi necessari per l'accesso con smart card.

![Diagramma che Mostra gli elementi e i processi necessari per l'accesso con smart card](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Architettura del provider di credenziali smart card**

Quando si usa una smart card anziché una password, una coppia di chiavi pubblica/privata archiviata nella smart card dell'utente viene sostituita dalla chiave privata condivisa, derivata dalla password dell'utente. La chiave privata viene archiviata solo sulla smart card. La chiave pubblica può essere resa disponibile a tutti gli utenti con cui il proprietario desidera scambiare informazioni riservate.

Per ulteriori informazioni sul processo di accesso tramite smart card in Windows, vedere funzionamento dell'accesso tramite [Smart Card in Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Accesso biometrico
Un dispositivo viene usato per acquisire e compilare una caratteristica digitale di un artefatto, ad esempio un'impronta digitale. Questa rappresentazione digitale viene quindi confrontata con un campione dello stesso artefatto e quando le due vengono confrontate correttamente, l'autenticazione può verificarsi. I computer che eseguono uno dei sistemi operativi indicati nell'elenco **si applica a** all'inizio di questo argomento possono essere configurati in modo da accettare questo tipo di accesso. Tuttavia, se l'accesso biometrico è configurato solo per l'accesso locale, l'utente deve presentare le credenziali del dominio per l'accesso a un dominio Active Directory.

## <a name="additional-resources"></a>Risorse aggiuntive
Per informazioni sul modo in cui Windows gestisce le credenziali inviate durante il processo di accesso, vedere [gestione delle credenziali nell'autenticazione di Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Panoramica tecnica dell'accesso e dell'autenticazione di Windows](https://technet.microsoft.com/library/dn169029.aspx)


