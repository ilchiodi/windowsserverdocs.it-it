---
title: Opzioni di accesso utente con Windows Admin Center
description: Opzioni di accesso utente e provider di identità con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2019
ms.locfileid: "9151996"
---
# Opzioni di accesso utente con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Quando viene distribuito in Windows Server, Windows Admin Center offre un punto centralizzato di gestione per l'ambiente server. Controllando l'accesso a Windows Admin Center, è possibile migliorare la sicurezza della tua orizzontale di gestione.

## Ruoli di accesso gateway

Windows Admin Center definisce due ruoli per l'accesso per il servizio gateway: gli utenti di gateway e gli amministratori di gateway.

> [!NOTE]
> Accesso al gateway non implica l'accesso ai server di destinazione visibile dal gateway. Per gestire un server di destinazione, un utente deve connettersi con le credenziali con privilegi di amministratore nel server di destinazione.

**Gli utenti di gateway** possono connettersi al servizio gateway Windows Admin Center per gestire i server tramite il gateway, ma non possono modificare le autorizzazioni di accesso né il meccanismo di autenticazione usato per autenticare al gateway.

**Gli amministratori di gateway** possono configurare chi ha accesso anche il modo in cui gli utenti l'autenticazione al gateway.

>[!NOTE]
> Se non sono presenti gruppi di accesso definiti in Windows Admin Center, i ruoli riflettono l'accesso al server gateway Windows. 

[Configurare l'accesso di utenti e amministratori di gateway in Windows Admin Center.](../configure/user-access-control.md)

## Opzioni del provider di identità

Gli amministratori di gateway scegliere una delle seguenti:

 - [Gruppi di computer Active Directory o locale](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory come provider di identità per Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### Autenticazione della smart card

Quando si utilizza Active Directory o gruppi di computer locale come provider di identità, è possibile applicare l'autenticazione con smart card da richiedere agli utenti che accedono a Windows Admin Center per essere membri dei gruppi di sicurezza basata su smart card aggiuntive. [Configurare l'autenticazione con smart card in Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### Accesso condizionale e l'autenticazione a più fattori

Specificando l'autenticazione di Azure AD per il gateway, è possibile sfruttare le funzionalità di sicurezza aggiuntive come accesso condizionale e autenticazione a più fattori fornita da Azure AD. [Altre informazioni sulla configurazione di accesso condizionale in Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## Controllo dell'accesso basato sui ruoli

Per impostazione predefinita, gli utenti richiedono privilegi di amministratore locale completo nei computer che desiderano gestire utilizzando Windows Admin Center.
Ciò consente loro di connettersi al computer in remoto e garantisce che dispone di autorizzazioni sufficienti per visualizzare e modificare le impostazioni di sistema.
Tuttavia, alcuni utenti potrebbero non essere necessario accedere senza restrizioni per il computer per eseguire le operazioni.
È possibile utilizzare **il controllo dell'accesso basato sui ruoli** in Windows Admin Center per fornire tali utenti con accesso limitato alla macchina invece di rendere inseriscono degli amministratori locali completo.

Controllo dell'accesso basato sui ruoli in Windows Admin Center funziona configurando ogni server gestito con un endpoint di PowerShell [(Just Enough Administration)](https://aka.ms/jeadocs) .
Questo endpoint definisce i ruoli, inclusi gli aspetti del sistema di ogni ruolo è autorizzato a gestire e gli utenti che sono assegnati al ruolo.
Quando un utente connette all'endpoint con restrizioni, viene creato un account amministratore temporaneo locale per gestire il sistema conto del cliente.
Ciò garantisce che anche strumenti che non hanno il proprio modello di delega comunque possono essere gestiti con Windows Admin Center.
L'account temporaneo viene rimosso automaticamente quando l'utente smette di gestione dei computer con Windows Admin Center.

Quando un utente si connette a un computer configurato con il controllo dell'accesso basato sui ruoli, Windows Admin Center verrà controlla innanzitutto se sono un amministratore locale.
In caso affermativo, esse applicheranno l'esperienza completa di Windows Admin Center senza alcuna restrizione.
In caso contrario, Windows Admin Center controllerà se l'utente a cui appartiene a uno dei ruoli predefiniti.
Un utente viene detto hanno *accesso limitato* se appartengono a un ruolo di Windows Admin Center, ma non sono un amministratore completo.
Infine, se l'utente è un amministratore né un membro di un ruolo, essi verranno negati l'accesso per gestire il computer.

Controllo dell'accesso basato sui ruoli è disponibile per le soluzioni di gestione Server e Cluster di Failover.

### Ruoli disponibili

Windows Admin Center supporta i ruoli degli utenti finali seguenti:

Nome del ruolo | Destinazione d'uso
----------|-------------
Administrators | Consente agli utenti di usare la maggior parte delle funzionalità in Windows Admin Center senza concedere l'accesso a Desktop remoto o PowerShell. Questo ruolo è utile per gli scenari di "saltare server" in cui si desidera limitare i punti di ingresso gestione in un computer.
Utilità per la lettura | Consente agli utenti di visualizzare informazioni e impostazioni nel server, ma non apportare modifiche.
Amministratori di Hyper-V | Consente agli utenti di apportare modifiche alle macchine virtuali Hyper-V e commutatori, ma limita le altre funzionalità di accesso in sola lettura.

Le estensioni predefinite seguenti ridotta funzionalità quando si connette a un utente con accesso limitato:

- File (il caricamento di file o download)
- PowerShell (non disponibile)
- Desktop remoto (non disponibile)
- Replica archiviazione (non disponibile)

In questo momento, non è possibile creare ruoli personalizzati per la tua organizzazione, ma puoi scegliere quali utenti sono autorizzati a ogni ruolo.

### Preparazione per il controllo dell'accesso basato sui ruoli

Per sfruttare gli account locali temporanei, ogni computer di destinazione deve essere configurato per supportare il controllo dell'accesso basato sui ruoli in Windows Admin Center.
Il processo di configurazione prevede l'installazione di un endpoint Just Enough Administration e script di PowerShell nel computer con configurazione dello stato desiderato.

Se hai solo pochi computer, è possibile applicare facilmente la configurazione singolarmente per ogni computer tramite la pagina di controllo di accesso basato sui ruoli in Windows Admin Center.
Quando configuri controllo dell'accesso basato sui ruoli in un computer singolo, gruppi di sicurezza locali vengono creati per controllare l'accesso a ogni ruolo.
È possibile concedere l'accesso a utenti o altri gruppi di sicurezza aggiungendoli come membri dei gruppi di sicurezza di ruolo.

Per una distribuzione a livello aziendale su più computer, puoi scaricare lo script di configurazione dal gateway e distribuirla ai computer tramite un server di pull di configurazione dello stato desiderato, automazione Azure o il strumenti di gestione preferita.
