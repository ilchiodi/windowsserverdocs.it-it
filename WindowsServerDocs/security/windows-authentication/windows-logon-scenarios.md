---
title: Scenari di accesso di Windows
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828182"
---
# <a name="windows-logon-scenarios"></a>Scenari di accesso di Windows

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento di riferimento per i professionisti IT riepiloga l'accesso di Windows comune e scenari di accesso.

I sistemi operativi Windows richiedono tutti gli utenti per accedere al computer con un account valido per l'accesso locale e le risorse di rete. I computer basati su Windows proteggere le risorse implementando il processo di accesso, in cui gli utenti vengono autenticati. Dopo che un utente viene autenticato, le tecnologie di controllo di accesso e autorizzazione implementano la seconda fase della protezione delle risorse: determinazione delle autorizzazioni se l'utente autenticato per accedere a una risorsa.

Il contenuto di questo argomento si applica a versioni di Windows indicate nel **si applica a** elenco all'inizio di questo argomento.

Inoltre, applicazioni e servizi possono richiedere agli utenti di accedere per visualizzare le risorse che sono disponibili per l'applicazione o servizio. Il processo di accesso è simile al processo di accesso, che sono necessari un account valido e le credenziali corrette, ma le informazioni di accesso vengono archiviate nel database Gestione Account di protezione (SAM) nel computer locale e in Active Directory dove applicabile. Account e credenziali le informazioni di accesso sono gestite dall'applicazione o al servizio e, facoltativamente, possono essere archiviate in locale nella casella di sicurezza delle credenziali.

Per comprendere il funzionamento dell'autenticazione, vedere [concetti di autenticazione di Windows](windows-authentication-concepts.md).

Questo argomento descrive gli scenari seguenti:

-   [Accesso interattivo](#BKMK_InteractiveLogon)

-   [Accesso alla rete](#BKMK_NetworkLogon)

-   [Accesso con smart card](#BKMK_SmartCardLogon)

-   [Accesso biometrico](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Accesso interattivo
Il processo di accesso inizia quando un utente immette le credenziali nella finestra di dialogo Immissione credenziali, o quando viene inserita una smart card nel lettore di smart card o quando l'utente interagisce con un dispositivo biometrico. Gli utenti possono eseguire un accesso interattivo con un account utente locale o un account di dominio per accedere a un computer.

Il diagramma seguente mostra gli elementi di accesso interattivo e processo di accesso.

![Diagramma che mostra gli elementi di accesso interattivo e processo di accesso](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Architettura dell'autenticazione Client Windows**

### <a name="BKMK_LocaDomainLogon"></a>Accesso locali e di dominio
Credenziali di cui dispone l'utente per l'accesso a un dominio contengono tutti gli elementi necessari per l'accesso locale, ad esempio nome dell'account e password o certificato e informazioni di dominio Active Directory. Il processo di conferma l'identificazione dell'utente nel database di sicurezza nel computer locale dell'utente o a un dominio di Active Directory. Questo processo di accesso obbligatorio non può essere disattivato per gli utenti in un dominio.

Gli utenti possono eseguire un accesso interattivo a un computer in uno dei due modi:

-   In locale, quando l'utente ha accesso fisico diretto per il computer o quando il computer fa parte di una rete di computer.

    L'accesso locale concede un'autorizzazione utente per accedere alle risorse di Windows nel computer locale. L'accesso locale richiede che l'utente abbia un account utente in Gestione account di sicurezza (SAM) nel computer locale. Il modulo SAM protegge e gestisce informazioni sull'utente e gruppo sotto forma di account di sicurezza archiviate nel Registro di sistema computer locale. Il computer può avere accesso alla rete, ma non è obbligatorio. Informazioni di appartenenza a gruppi e account utente locale vengano usate per gestire l'accesso alle risorse locali.

    Un utente l'autorizzazione per accedere alle risorse di Windows nel computer locale, oltre alle eventuali risorse nei computer in rete come definito dal token di accesso della credenziale concede l'accesso alla rete. L'accesso locale sia un account di accesso di rete richiedono che l'utente ha un account utente in Gestione account di sicurezza (SAM) nel computer locale. Informazioni di appartenenza a gruppi e account utente locale viene usate per gestire l'accesso alle risorse locali e il token di accesso per l'utente definisce quali risorse sono accessibili nel computer in rete.

    L'accesso locale e un account di accesso di rete non sono sufficienti per concedere l'autorizzazione utente e computer per accedere e usare risorse di dominio.

-   In modalità remota, tramite servizi Terminal o Servizi Desktop remoto (RDS), nel qual caso l'account di accesso è ulteriormente qualificato come remote interactive.

Dopo un accesso interattivo, Windows esegue le applicazioni per conto dell'utente e l'utente può interagire con tali applicazioni.

L'accesso locale concede un'autorizzazione utente per accedere alle risorse nel computer locale o le risorse nel computer in rete. Se il computer è unito a un dominio, quindi la funzionalità di Winlogon tenta di accedere a tale dominio.

Accesso a un dominio concede un'autorizzazione utente per l'accesso locale e le risorse di dominio. Accesso a un dominio richiede che l'utente abbia un account utente in Active Directory. Il computer deve avere un account nel dominio di Active Directory ed essere fisicamente connesso alla rete. Gli utenti devono inoltre disporre dei diritti utente per accedere a un computer locale o in un dominio. Informazioni sull'account utente di dominio e informazioni di appartenenza vengono utilizzati per gestire l'accesso al dominio e le risorse locali.

### <a name="BKMK_RemoteLogon"></a>Accesso remoto.
In Windows, l'accesso a un altro computer tramite accesso remoto si basa sul Remote Desktop Protocol (RDP). Poiché l'utente deve già disporre correttamente connesso al computer client prima di tentare una connessione remota, aver completato correttamente i processi di accesso interattivo.

RDP gestisce le credenziali immesse dall'utente tramite il Client Desktop remoto. Tali credenziali sono destinate a computer di destinazione e l'utente deve avere un account per il computer di destinazione. Inoltre, il computer di destinazione deve essere configurato per accettare una connessione remota. Le credenziali del computer di destinazione vengono inviate per tentare di eseguire il processo di autenticazione. Se l'autenticazione ha esito positivo, l'utente è connesso a locale e le risorse di rete che sono accessibili tramite le credenziali fornite.

## <a name="BKMK_NetworkLogon"></a>Accesso alla rete
Accesso alla rete è utilizzabile solo una volta eseguita l'autenticazione utente, servizio o computer. Durante l'accesso alla rete, il processo non utilizza le finestre di dialogo Immissione credenziali per raccogliere i dati. Al contrario, stabilita in precedenza le credenziali o viene usato un altro metodo per raccogliere le credenziali. Questo processo viene confermata l'identità dell'utente a qualsiasi servizio di rete che l'utente sta provando ad accedere. Questo processo è in genere invisibile all'utente a meno che non devono essere forniti credenziali alternative.

Per fornire questo tipo di autenticazione, il sistema di sicurezza include questi meccanismi di autenticazione:

-   Protocollo Kerberos versione 5

-   Certificati di chiave pubblica

-   Secure Socket Layer/Transport Layer Security (TLS/SSL)

-   Digest

-   NTLM, per garantire la compatibilità con sistemi basati su Microsoft Windows NT 4.0

Per informazioni sugli elementi e i processi, vedere il diagramma di accesso interattivo precedente.

## <a name="BKMK_SmartCardLogon"></a>Accesso con smart card
Le smart card è utilizzabile per accedere solo agli account di dominio, gli account non locali. Autenticazione con smart card richiede l'uso del protocollo di autenticazione Kerberos. Introdotti in Windows 2000 Server, nei sistemi operativi basati su Windows un'estensione di chiave pubblica per Kerberos viene implementata la richiesta di autenticazione iniziale del protocollo. A differenza di crittografia a chiave segreta condivisa, crittografia a chiave pubblica è asimmetrica, vale a dire, sono necessarie due chiavi diverse: uno per la crittografia, un'altra per decrittografare. Insieme, le chiavi necessarie per eseguire entrambe le operazioni costituiscono una coppia di chiavi pubblica/privata.

Per avviare una sessione di accesso tipico, un utente deve dimostrare la propria identità, fornendo informazioni note solo all'utente e l'infrastruttura sottostante di protocollo Kerberos. Le informazioni segrete sono una chiave crittografica condivisa derivata dalla password dell'utente. Una chiave privata condivisa è simmetrica, ovvero la stessa chiave viene utilizzata per la crittografia e decrittografia.

Il diagramma seguente mostra gli elementi e i processi necessari per l'accesso con smart card.

![Diagramma che mostra gli elementi e i processi necessari per l'accesso con smart card](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Architettura del provider di credenziali della Smart Card**

Quando una smart card viene usata invece di una password, una coppia di chiavi pubblica/privata archiviata sulla smart card dell'utente verrà sostituita con la chiave privata condivisa, che viene derivata dalla password dell'utente. La chiave privata viene archiviata solo sulla smart card. La chiave pubblica può essere resa disponibile a tutti gli utenti con cui il proprietario desidera che per lo scambio di informazioni riservate.

Per altre informazioni sul processo di accesso della smart card in Windows, vedere [Accedi della smart card funzionamento in Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Accesso biometrico
Un dispositivo viene usato per acquisire e compilare una caratteristica digitale di un elemento, ad esempio un'impronta digitale. Questa rappresentazione digitale viene quindi confrontato con un esempio dell'elemento stesso e quando i due vengono confrontati correttamente, può verificarsi l'autenticazione. I computer che eseguono uno qualsiasi dei sistemi operativi indicati nel **si applica a** elenco all'inizio di questo argomento può essere configurato per accettare il modulo di accesso. Tuttavia, se l'accesso biometrico è configurata solo per l'accesso locale, l'utente deve presentare le credenziali di dominio quando si accede a un dominio Active Directory.

## <a name="additional-resources"></a>Risorse aggiuntive
Per informazioni sulle modalità di gestione delle credenziali presentate durante il processo di accesso di Windows, vedere [la gestione delle credenziali di autenticazione basata su Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Panoramica tecnica dell'accesso di Windows e autenticazione](https://technet.microsoft.com/library/dn169029.aspx)


