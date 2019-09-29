---
title: Opzioni di accesso utente con l'interfaccia di amministrazione di Windows
description: Opzioni di accesso utente e provider di identità con il centro di amministrazione di Windows (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356923"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opzioni di accesso utente con l'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Quando viene distribuito in Windows Server, l'interfaccia di amministrazione di Windows offre un punto di gestione centralizzato per l'ambiente server. Controllando l'accesso all'interfaccia di amministrazione di Windows, è possibile migliorare la sicurezza del panorama di gestione.

## <a name="gateway-access-roles"></a>Ruoli di accesso al gateway

L'interfaccia di amministrazione di Windows definisce due ruoli per l'accesso al servizio gateway: utenti del gateway e amministratori del gateway.

> [!NOTE]
> L'accesso al gateway non implica l'accesso ai server di destinazione visibili dal gateway. Per gestire un server di destinazione, è necessario che l'utente si connetta con credenziali con privilegi amministrativi nel server di destinazione.

**Gli utenti del gateway** possono connettersi al servizio gateway dell'interfaccia di amministrazione di Windows per gestire i server tramite tale gateway, ma non possono modificare le autorizzazioni di accesso né il meccanismo di autenticazione usato per autenticare il gateway.

Gli **amministratori del gateway** possono configurare chi ottiene l'accesso e il modo in cui gli utenti eseguiranno l'autenticazione al gateway.

>[!NOTE]
> Se nell'interfaccia di amministrazione di Windows non sono definiti gruppi di accesso, i ruoli riflettono l'accesso dell'account di Windows al server gateway. 

[Configurare l'accesso dell'utente e dell'amministratore del gateway nell'interfaccia di amministrazione di Windows.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opzioni del provider di identità

Gli amministratori del gateway possono scegliere una delle seguenti opzioni:

 - [Gruppi di computer Active Directory/locali](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory come provider di identità per l'interfaccia di amministrazione di Windows](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticazione smart card

Quando si usa Active Directory o gruppi di computer locali come provider di identità, è possibile applicare l'autenticazione con smart card richiedendo agli utenti che accedono all'interfaccia di amministrazione di Windows di essere membri di altri gruppi di sicurezza basati su smart card. [Configurare l'autenticazione con smart card nell'interfaccia di amministrazione di Windows.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Accesso condizionale e autenticazione a più fattori

Richiedendo l'autenticazione Azure AD per il gateway, è possibile sfruttare funzionalità di sicurezza aggiuntive, ad esempio l'accesso condizionale e l'autenticazione a più fattori fornita da Azure AD. [Altre informazioni sulla configurazione dell'accesso condizionale con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

Per impostazione predefinita, gli utenti richiedono privilegi di amministratore locale completi nei computer che desiderano gestire tramite l'interfaccia di amministrazione di Windows.
In questo modo, è possibile connettersi al computer in remoto e assicurarsi che dispongano di autorizzazioni sufficienti per visualizzare e modificare le impostazioni di sistema.
Tuttavia, è possibile che alcuni utenti non debbano accedere senza limitazioni al computer per eseguire i propri processi.
È possibile usare il **controllo degli accessi in base al ruolo** nell'interfaccia di amministrazione di Windows per fornire a tali utenti un accesso limitato al computer invece di renderli completi per gli amministratori locali.

Il controllo degli accessi in base al ruolo nell'interfaccia di amministrazione di Windows consente di configurare ogni server gestito con un endpoint di [amministrazione sufficiente](https://aka.ms/jeadocs) per PowerShell.
Questo endpoint definisce i ruoli, inclusi gli aspetti del sistema che ciascun ruolo è autorizzato a gestire e gli utenti assegnati al ruolo.
Quando un utente si connette all'endpoint con restrizioni, viene creato un account amministratore locale temporaneo per gestire il sistema per loro conto.
In questo modo, anche gli strumenti che non dispongono di un modello di delega personalizzato possono comunque essere gestiti con l'interfaccia di amministrazione di Windows.
L'account temporaneo viene rimosso automaticamente quando l'utente interrompe la gestione del computer tramite l'interfaccia di amministrazione di Windows.

Quando un utente si connette a un computer configurato con il controllo degli accessi in base al ruolo, il centro di amministrazione di Windows verificherà prima di tutto se è un amministratore locale.
In caso affermativo, riceveranno l'esperienza completa di amministrazione di Windows senza alcuna restrizione.
In caso contrario, il centro di amministrazione di Windows verificherà se l'utente appartiene a uno dei ruoli predefiniti.
Un utente ha *l'accesso limitato* se appartiene a un ruolo di centro di amministrazione di Windows, ma non è un amministratore completo.
Infine, se l'utente non è né un amministratore né un membro di un ruolo, verrà negato l'accesso per gestire il computer.

Il controllo degli accessi in base al ruolo è disponibile per le soluzioni di Server Manager e cluster di failover.

### <a name="available-roles"></a>Ruoli disponibili

L'interfaccia di amministrazione di Windows supporta i seguenti ruoli utente finali:

Nome del ruolo | Destinazione d'uso
----------|-------------
Administrators | Consente agli utenti di usare la maggior parte delle funzionalità dell'interfaccia di amministrazione di Windows senza concedere loro l'accesso a Desktop remoto o PowerShell. Questo ruolo è ideale per gli scenari "Jump server" in cui si desidera limitare i punti di ingresso di gestione in un computer.
Readers | Consente agli utenti di visualizzare le informazioni e le impostazioni nel server, ma non di apportare modifiche.
Amministratori Hyper-V | Consente agli utenti di apportare modifiche alle macchine virtuali e ai commutatori Hyper-V, ma limita le altre funzionalità per l'accesso in sola lettura.

Le estensioni predefinite seguenti hanno una funzionalità ridotta quando un utente si connette con accesso limitato:

- File (nessun caricamento o download di file)
- PowerShell (non disponibile)
- Desktop remoto (non disponibile)
- Replica archiviazione (non disponibile)

Al momento non è possibile creare ruoli personalizzati per l'organizzazione, ma è possibile scegliere a quali utenti viene concesso l'accesso a ogni ruolo.

### <a name="preparing-for-role-based-access-control"></a>Preparazione per il controllo degli accessi in base al ruolo

Per sfruttare gli account locali temporanei, ogni computer di destinazione deve essere configurato per supportare il controllo degli accessi in base al ruolo nell'interfaccia di amministrazione di Windows.
Il processo di configurazione prevede l'installazione di script di PowerShell e un endpoint di amministrazione sufficiente nel computer con la configurazione dello stato desiderato.

Se si dispone solo di pochi computer, è possibile applicare facilmente la configurazione singolarmente a ogni computer usando la pagina di controllo degli accessi in base al ruolo nell'interfaccia di amministrazione di Windows.
Quando si configura il controllo degli accessi in base al ruolo in un singolo computer, vengono creati gruppi di sicurezza locali per controllare l'accesso a ogni ruolo.
È possibile concedere l'accesso a utenti o altri gruppi di sicurezza aggiungendoli come membri dei gruppi di sicurezza del ruolo.

Per una distribuzione a livello aziendale su più computer, è possibile scaricare lo script di configurazione dal gateway e distribuirlo ai computer usando un server di pull di configurazione dello stato desiderato, automazione di Azure o gli strumenti di Gestione Preferiti.
