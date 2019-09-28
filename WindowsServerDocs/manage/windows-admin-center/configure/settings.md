---
title: Impostazioni
description: Informazioni sulle impostazioni nell'interfaccia di amministrazione di Windows (progetto Honolulu). Le impostazioni utente consentono agli utenti di modificare la lingua/area geografica e altre preferenze. Le impostazioni del gateway consentono agli amministratori di configurare il gateway.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: e0fd6618f275058d4e22fe9abb9e484d4752ac9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407055"
---
# <a name="windows-admin-center-settings"></a>Impostazioni dell'interfaccia di amministrazione di Windows

> Si applica a: Windows Admin Center

Le impostazioni dell'interfaccia di amministrazione di Windows sono costituite da impostazioni a livello di utente e di gateway. Una modifica a un'impostazione a livello di utente influisca solo sul profilo dell'utente corrente, mentre una modifica a un'impostazione a livello di gateway ha effetto su tutti gli utenti del gateway di Windows Admin Center.

## <a name="user-settings"></a>Impostazioni utente

Le impostazioni a livello di utente sono costituite dalle seguenti sezioni:

- Account
- Personalization
- Lingua/area geografica
- Suggerimenti
- Avanzate

Nella scheda **account** gli utenti possono esaminare le credenziali usate per l'autenticazione nell'interfaccia di amministrazione di Windows. Se Azure AD è configurato come provider di identità, l'utente può disconnettersi dall'account Azure AD da questa scheda.

Nella scheda **personalizzazione** gli utenti possono selezionare un tema dell'interfaccia utente scuro.

Nella scheda **lingua/area** , gli utenti possono modificare i formati della lingua e dell'area visualizzati dall'interfaccia di amministrazione di Windows.

Nella scheda **suggerimenti** gli utenti possono visualizzare o disabilitare i suggerimenti sui servizi di Azure e le nuove funzionalità.

La scheda **Avanzate** fornisce agli sviluppatori di estensioni di Windows Admin Center funzionalità aggiuntive.

## <a name="gateway-settings"></a>Impostazioni del gateway

Le impostazioni a livello di gateway sono costituite dalle seguenti sezioni:

- Estensioni
- Accesso
- Azure
- Connessioni condivise

Solo gli amministratori del gateway sono in grado di visualizzare e modificare queste impostazioni. Le modifiche apportate a queste impostazioni cambiano la configurazione del gateway e influiscono su tutti gli utenti del gateway dell'interfaccia di amministrazione di Windows.

Nella scheda **estensioni** gli amministratori possono installare, disinstallare o aggiornare le estensioni del gateway. [Altre informazioni sulle estensioni.](using-extensions.md)

La scheda **accesso** consente agli amministratori di configurare gli utenti che possono accedere al gateway dell'interfaccia di amministrazione di Windows, nonché il provider di identità usato per autenticare gli utenti. [Altre informazioni sul controllo dell'accesso al gateway.](user-access-control.md)

Dalla scheda **Azure** gli amministratori possono registrare il gateway con Azure per abilitare le [funzionalità di integrazione di Azure](azure-integration.md) nell'interfaccia di amministrazione di Windows.

Utilizzando la scheda **connessioni condivise** , gli amministratori possono configurare un singolo elenco di connessioni da condividere tra tutti gli utenti del gateway dell'interfaccia di amministrazione di Windows. [Altre informazioni sulla configurazione delle connessioni una volta per tutti gli utenti di un gateway.](shared-connections.md)