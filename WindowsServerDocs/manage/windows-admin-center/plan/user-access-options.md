---
title: Opzioni di accesso utente con Windows Admin Center
description: Opzioni di accesso utente e provider di identità con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71356923"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opzioni di accesso utente con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Se distribuito in Windows Server, Windows Admin Center offre un punto di gestione centralizzato per l'ambiente server. Controllando l'accesso a Windows Admin Center, puoi migliorare la sicurezza nel campo della gestione.

## <a name="gateway-access-roles"></a>Ruoli di accesso al gateway

Windows Admin Center definisce due ruoli per l'accesso al servizio gateway: utenti del gateway e amministratori del gateway.

> [!NOTE]
> L'accesso al gateway non implica l'accesso ai server di destinazione visibili dal gateway. Per gestire un server di destinazione, è necessario che l'utente si connetta usando credenziali con privilegi amministrativi nel server di destinazione.

Gli **utenti del gateway** possono connettersi al servizio gateway di Windows Admin Center per gestire i server attraverso tale gateway, ma non possono modificare le autorizzazioni di accesso o il meccanismo usato per l'autenticazione al gateway.

Gli **amministratori del gateway** possono configurare gli utenti autorizzati ad accedere al servizio gateway e la relativa modalità di autenticazione.

>[!NOTE]
> Se in Windows Admin Center non sono definiti gruppi di accesso, i ruoli riflettono l'accesso dell'account Windows al server gateway. 

[Configura l'accesso degli utenti e degli amministratori del gateway in Windows Admin Center.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opzioni per il provider di identità

Gli amministratori del gateway possono scegliere una delle opzioni seguenti:

 - [Active Directory o gruppi di computer locali](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory come provider di identità per Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticazione tramite smart card

Quando usi Active Directory o gruppi di computer locali come provider di identità, puoi applicare l'autenticazione tramite smart card richiedendo agli utenti che accedono a Windows Admin Center di essere membri di altri gruppi di sicurezza basati su smart card. [Configura l'autenticazione tramite smart card in Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Accesso condizionale e autenticazione a più fattori

Richiedendo l'autenticazione Azure AD per il gateway, puoi sfruttare funzionalità di sicurezza aggiuntive, come l'accesso condizionale e l'autenticazione a più fattori, fornite da Azure AD. [Altre informazioni sulla configurazione dell'accesso condizionale con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Controllo di accesso in base ai ruoli

Per impostazione predefinita, gli utenti richiedono privilegi di amministratore locale completi nei computer che vogliono gestire tramite Windows Admin Center.
In questo modo, possono connettersi al computer in remoto ed essere certi di disporre di autorizzazioni sufficienti per visualizzare e modificare le impostazioni di sistema.
Tuttavia, è possibile che alcuni utenti non abbiano necessità di accedere senza limitazioni al computer per svolgere le loro attività.
Puoi usare il **controllo degli accessi in base al ruolo** in Windows Admin Center per fornire a tali utenti l'accesso limitato al computer invece di concedere loro i privilegi completi di amministratore locale.

Il controllo degli accessi in base al ruolo in Windows Admin Center funziona configurando ogni server gestito con un endpoint [JEA (Just Enough Administration)](https://aka.ms/jeadocs) di PowerShell.
Questo endpoint definisce i ruoli, inclusi gli aspetti del sistema che ciascun ruolo è autorizzato a gestire e gli utenti assegnati al ruolo.
Quando un utente si connette all'endpoint con limitazioni, viene creato un account amministratore locale temporaneo per gestire il sistema per suo conto.
In questo modo, anche gli strumenti che non dispongono di un modello di delega specifico possono comunque essere gestiti con Windows Admin Center.
L'account temporaneo viene rimosso automaticamente quando l'utente smette di gestire il computer tramite Windows Admin Center.

Quando un utente si connette a un computer configurato con il controllo degli accessi in base al ruolo, Windows Admin Center verifica prima di tutto se è un amministratore locale.
In caso affermativo, l'utente riceve l'esperienza completa di Windows Admin Center senza alcuna limitazione.
In caso contrario, Windows Admin Center verifica se l'utente appartiene a uno dei ruoli predefiniti.
Si dice che un utente ha *accesso limitato* se appartiene a un ruolo di Windows Admin Center, ma non è un amministratore completo.
Se infine l'utente non è né un amministratore né un membro di un ruolo, gli viene negato l'accesso per la gestione del computer.

Il controllo degli accessi in base al ruolo è disponibile per le soluzioni di cluster di failover e Server Manager.

### <a name="available-roles"></a>Ruoli disponibili

Windows Admin Center supporta i ruoli seguenti per gli utenti finali:

Nome ruolo | Destinazione d'uso
----------|-------------
Administrators | Consente agli utenti di usare la maggior parte delle funzionalità di Windows Admin Center senza concedere loro l'accesso a Desktop remoto o PowerShell. Questo ruolo è appropriato per gli scenari con "jump server" in cui vuoi limitare i punti di ingresso di gestione in un computer.
Readers | Consente agli utenti di visualizzare le informazioni e le impostazioni nel server, ma non di apportare modifiche.
Amministratori Hyper-V | Consente agli utenti di apportare modifiche ai commutatori e alle macchine virtuali Hyper-V, ma limita altre funzionalità all'accesso in sola lettura.

Le estensioni predefinite seguenti hanno una funzionalità ridotta se un utente si connette con accesso limitato:

- File (nessun caricamento o download di file)
- PowerShell (non disponibile)
- Desktop remoto (non disponibile)
- Replica archiviazione (non disponibile)

In questa fase non puoi creare ruoli personalizzati per l'organizzazione, ma puoi scegliere a quali utenti viene concesso l'accesso a ogni ruolo.

### <a name="preparing-for-role-based-access-control"></a>Preparazione per il controllo degli accessi in base al ruolo

Per sfruttare gli account locali temporanei, ogni computer di destinazione deve essere configurato per supportare il controllo degli accessi in base al ruolo in Windows Admin Center.
Il processo di configurazione prevede l'installazione di script di PowerShell e un endpoint JEA (Just Enough Administration) nel computer con Desired State Configuration.

Se disponi solo di pochi computer, puoi applicare facilmente la configurazione singolarmente a ognuno di essi usando la pagina relativa al controllo degli accessi in base al ruolo in Windows Admin Center.
Quando configuri il controllo degli accessi in base al ruolo in un singolo computer, vengono creati gruppi di sicurezza locali per controllare l'accesso a ogni ruolo.
Puoi concedere l'accesso a utenti o altri gruppi di sicurezza aggiungendoli come membri dei gruppi di sicurezza del ruolo.

Per una distribuzione a livello aziendale in più computer, puoi scaricare lo script di configurazione dal gateway e distribuirlo ai computer usando un server di pull di Desired State Configuration, Automazione di Azure o gli strumenti di gestione che preferisci.
