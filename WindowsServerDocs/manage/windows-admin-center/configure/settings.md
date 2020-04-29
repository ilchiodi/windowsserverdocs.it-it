---
title: Impostazioni
description: Informazioni sulle impostazioni in Windows Admin Center (Project Honolulu). Le impostazioni utente consentono agli utenti di modificare la lingua o l'area geografica e altre preferenze. Le impostazioni del gateway consentono agli amministratori di configurare il gateway.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: e0fd6618f275058d4e22fe9abb9e484d4752ac9a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71407055"
---
# <a name="windows-admin-center-settings"></a>Impostazioni di Windows Admin Center

> Si applica a: Windows Admin Center

Le impostazioni di Windows Admin Center sono costituite da impostazioni a livello di utente e di gateway. Una modifica a un'impostazione a livello di utente influisce solo sul profilo dell'utente corrente, mentre una modifica a un'impostazione a livello di gateway ha effetto su tutti gli utenti del gateway di Windows Admin Center.

## <a name="user-settings"></a>Impostazioni utente

Le impostazioni a livello di utente sono costituite dalle sezioni seguenti:

- Account
- Personalization
- Lingua/Area geografica
- Suggerimenti
- Avanzate

Nella scheda **Account** gli utenti possono esaminare le credenziali usate per l'autenticazione a Windows Admin Center. Se Azure AD è configurato come provider di identità, l'utente può disconnettersi dall'account Azure AD usando questa scheda.

Nella scheda **Personalizzazione** gli utenti possono attivare o disattivare un tema dell'interfaccia utente scuro.

Nella scheda **Lingua/Area geografica** gli utenti possono modificare i formati della lingua e dell'area geografica visualizzati da Windows Admin Center.

Nella scheda **Suggerimenti** gli utenti possono attivare o disattivare i suggerimenti sui servizi di Azure e sulle nuove funzionalità.

La scheda **Avanzate** fornisce funzionalità aggiuntive agli sviluppatori di estensioni di Windows Admin Center.

## <a name="gateway-settings"></a>Impostazioni gateway

Le impostazioni a livello di gateway sono costituite dalle sezioni seguenti:

- Extensions
- Accesso
- Azure
- Connessioni condivise

Solo gli amministratori del gateway possono visualizzare e modificare queste impostazioni. Le modifiche apportate a queste impostazioni cambiano la configurazione del gateway e hanno effetto su tutti gli utenti del gateway di Windows Admin Center.

Nella scheda **Estensioni** gli amministratori possono installare, disinstallare o aggiornare le estensioni del gateway. [Altre informazioni sulle estensioni.](using-extensions.md)

La scheda **Accesso** consente agli amministratori di configurare gli utenti che possono accedere al gateway di Windows Admin Center, nonché il provider di identità usato per autenticare gli utenti. [Altre informazioni sul controllo dell'accesso al gateway.](user-access-control.md)

Dalla scheda **Azure** gli amministratori possono registrare il gateway con Azure per abilitare le [funzionalità di integrazione di Azure](azure-integration.md) in Windows Admin Center.

Usando la scheda **Connessioni condivise**, gli amministratori possono configurare un singolo elenco di connessioni da condividere tra tutti gli utenti del gateway di Windows Admin Center. [Altre informazioni sulla configurazione delle connessioni una volta per tutti gli utenti di un gateway.](shared-connections.md)