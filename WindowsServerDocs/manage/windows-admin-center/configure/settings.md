---
title: Impostazioni
description: Informazioni sulle impostazioni di Windows Admin Center (Project Honolulu). Le impostazioni utente consentono agli utenti di modificare la lingua/area geografica e altre preferenze. Impostazioni del gateway consentono agli amministratori di configurare il gateway.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d67a2c743900792353141186112cd09dbf780309
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296623"
---
# Impostazioni di Windows Admin Center

> Si applica a: Windows Admin Center

Windows Admin Center sono disponibili impostazioni a livello di utente e a livello di gateway. Una modifica un'impostazione a livello di utente interessa solo il profilo dell'utente corrente, mentre una modifica un'impostazione a livello di gateway influisce su tutti gli utenti in tale gateway Windows Admin Center.

## Impostazioni dell'utente

Le impostazioni a livello di utente sono costituite dalle sezioni seguenti:

- Account
- Personalization
- Lingua/area geografica
- Suggerimenti
- Avanzato

Nella scheda **dell'Account** , gli utenti possono esaminare le credenziali che sono usati per l'autenticazione di Windows Admin Center. Se Azure AD è configurato per essere il provider di identità, l'utente può accedere dal proprio account Azure AD da questa scheda.

Nella scheda **personalizzazione** , gli utenti possono attivare o disattivare a un tema scuro dell'interfaccia utente.

Nella scheda **Lingua/area geografica** , gli utenti possono modificare i formati di lingua e area geografica visualizzati da Windows Admin Center.

Nella scheda **suggerimenti** , gli utenti possono attivare o disattivare suggerimenti sulle nuove funzionalità e i servizi Azure.

Scheda **Avanzate** offre agli sviluppatori di estensione Windows Admin Center funzionalità aggiuntive.

## Impostazioni del gateway

Le impostazioni a livello di gateway sono costituite dalle sezioni seguenti:

- Estensioni
- Access
- Azure
- Connessioni condivise

Solo gli amministratori di gateway sono in grado di visualizzare e modificare queste impostazioni. Le modifiche a queste impostazioni modificare la configurazione del gateway e interessano tutti gli utenti del gateway Windows Admin Center.

Nella scheda **estensioni** , gli amministratori possono installare, disinstallare o aggiornare le estensioni di gateway. [Altre informazioni sulle estensioni.](using-extensions.md)

Scheda **accesso** consente agli amministratori di configurare quali utenti possono accedere al gateway Windows Admin Center, nonché il provider di identità usato per autenticare gli utenti. [Altre informazioni sul controllo dell'accesso al gateway.](user-access-control.md)

Nella scheda **Azure** , gli amministratori possono registrare il gateway con Azure per abilitare le [funzionalità di integrazione di Azure](azure-integration.md) in Windows Admin Center.

Usando la scheda **Connessioni condiviso** , gli amministratori possono configurare un unico elenco di connessioni per essere condiviso tra tutti gli utenti del gateway Windows Admin Center. [Ulteriori informazioni sulla configurazione delle connessioni una sola volta per tutti gli utenti di un gateway.](shared-connections.md)