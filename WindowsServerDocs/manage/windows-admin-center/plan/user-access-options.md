---
title: Opzioni di accesso utente con Windows Admin Center
description: Opzioni di accesso utente e provider di identità con Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825892"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opzioni di accesso utente con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Quando distribuito in Windows Server, Windows Admin Center fornisce un punto di gestione per l'ambiente server centralizzato. Controllando l'accesso a Windows Admin Center, è possibile migliorare la sicurezza del panorama applicativo di gestione.

## <a name="gateway-access-roles"></a>Ruoli di accesso di gateway

Windows Admin Center definisce due ruoli per l'accesso al servizio del gateway: gateway utenti e amministratori di gateway.

> [!NOTE]
> Accesso al gateway non implica l'accesso ai server di destinazione visibili da parte del gateway. Per gestire un server di destinazione, è necessario connettersi con credenziali che dispongono di privilegi amministrativi nel server di destinazione.

**Gli utenti di gateway** possono connettersi al servizio del gateway Windows Admin Center per gestire i server tramite tale gateway, ma non possono modificare le autorizzazioni di accesso né il meccanismo di autenticazione usato per l'autenticazione del gateway.

**Gli amministratori di gateway** configurabili chi ottiene anche l'accesso in modalità di autenticazione degli utenti al gateway.

>[!NOTE]
> Se non sono presenti gruppi di accesso definiti in Windows Admin Center, i ruoli verranno applicata l'accesso all'account di Windows per il server gateway. 

[Configurare l'accesso di utenti e amministratori di gateway in Windows Admin Center.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opzioni del provider di identità

Gli amministratori di gateway possono scegliere le operazioni seguenti:

 - [Gruppi di computer Active Directory o locale](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory come provider di identità per Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticazione tramite smart card

Quando si usano i gruppi di computer locale o Active Directory come provider di identità, è possibile imporre l'autenticazione della smart card richiedendo agli utenti che accedono a Windows Admin Center per essere un membro dei gruppi di sicurezza aggiuntiva basata su smart card. [Configurare l'autenticazione della smart card in Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Accesso condizionale e multi-factor authentication

Richiedendo l'autenticazione di Azure AD per il gateway, è possibile sfruttare le funzionalità di sicurezza aggiuntive, ad esempio l'accesso condizionale e multi-factor authentication forniti da Azure AD. [Altre informazioni sulla configurazione dell'accesso condizionale con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Controllo di accesso in base ai ruoli

Per impostazione predefinita, gli utenti richiedono privilegi di amministratore locale completo nei computer che desiderano gestire tramite Windows Admin Center.
Questo consente loro di connettersi in remoto alla macchina e garantisce che dispone di autorizzazioni sufficienti per visualizzare e modificare le impostazioni di sistema.
Tuttavia, alcuni utenti non potrebbe essere necessario un accesso illimitato al computer per svolgere il lavoro.
È possibile usare **controllo degli accessi in base al ruolo** in Windows Admin Center per fornire tali utenti con accesso limitato al computer anziché lasciare che tali amministratori locali completo.

Controllo degli accessi in base al ruolo in Windows Admin Center funziona tramite la configurazione di ogni server gestito con un PowerShell [Just Enough Administration](https://aka.ms/jeadocs) endpoint.
Questo endpoint consente di definire i ruoli, incluse informazioni sugli aspetti del sistema di ogni ruolo può gestire e quali utenti vengono assegnati al ruolo.
Quando un utente si connette all'endpoint con restrizioni, un account amministratore locale temporaneo viene creato per gestire il sistema per loro conto.
Ciò garantisce che anche gli strumenti che non hanno il proprio modello di delega possono comunque essere gestiti con Windows Admin Center.
L'account temporaneo viene rimosso automaticamente quando l'utente smette di gestione della macchina virtuale tramite Windows Admin Center.

Quando un utente si connette a un computer configurato con controllo degli accessi in base al ruolo, Windows Admin Center verranno prima di tutto verificare se sono un amministratore locale.
In tal caso, si riceverà l'esperienza completa di Windows Admin Center senza alcuna restrizione.
In caso contrario, Windows Admin Center controllerà se l'utente appartiene a uno dei ruoli predefiniti.
Si dice che un utente ha *accesso limitato* se appartengono a un ruolo Windows Admin Center ma che non sono un amministratore completo.
Infine, se l'utente non amministratore né un membro di un ruolo, essi verrà negati l'accesso per gestire la macchina.

Controllo degli accessi in base al ruolo è disponibile per le soluzioni di gestione di Server e Cluster di Failover.

### <a name="available-roles"></a>Ruoli disponibili

Windows Admin Center supporta i seguenti ruoli degli utenti finali:

Nome ruolo | Destinazione d'uso
----------|-------------
Administrators | Consente agli utenti di usare la maggior parte delle funzionalità in Windows Admin Center senza concedere loro l'accesso a Desktop remoto o PowerShell. Questo ruolo è ideale per scenari "jump server" in cui si desidera limitare i punti di ingresso di gestione in un computer.
Readers | Consente agli utenti di visualizzare informazioni e impostazioni nel server, ma non apportare modifiche.
Amministratori Hyper-V | Consente agli utenti di apportare modifiche alle macchine virtuali Hyper-V e commutatori, ma limita altre funzionalità per l'accesso di sola lettura.

Le estensioni predefinite seguenti funzionalità ridotta quando si connette un utente con accesso limitato:

- File (senza caricamento del file o il download)
- PowerShell (non disponibile)
- Desktop remoto (non disponibile)
- Replica archiviazione (non disponibile)

A questo punto, è possibile creare ruoli personalizzati per l'organizzazione, ma è possibile scegliere quali utenti hanno accesso a ogni ruolo.

### <a name="preparing-for-role-based-access-control"></a>Preparazione per il controllo di accesso basato sui ruoli

Per sfruttare gli account locali temporanei, ogni computer di destinazione deve essere configurato per supportare il controllo di accesso basato sui ruoli in Windows Admin Center.
Il processo di configurazione comporta l'installazione degli script di PowerShell e un endpoint di Just Enough Administration nella macchina con Desired State Configuration.

Se si dispone solo di pochi computer, è possibile applicare facilmente la configurazione singolarmente in ogni computer utilizzando la pagina di controllo di accesso basato sui ruoli in Windows Admin Center.
Quando si imposta il controllo di accesso basato sui ruoli in un singolo computer, vengono creati gruppi di sicurezza locali per controllare l'accesso a ogni ruolo.
È possibile concedere l'accesso a utenti o altri gruppi di sicurezza aggiungendole come membri dei gruppi di sicurezza ruolo.

Per una distribuzione a livello aziendale su più computer, è possibile scaricare lo script di configurazione dal gateway e distribuirlo ai computer tramite un server di pull Desired State Configuration, automazione di Azure o degli strumenti di gestione preferiti.
