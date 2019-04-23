---
title: Impostazioni
description: Informazioni sulle impostazioni in Windows Admin Center (progetto Honolulu). Le impostazioni utente consentono agli utenti di modificare la lingua/area geografica e altre preferenze. Le impostazioni del gateway consentono agli amministratori di configurare il gateway.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1e1231500733f70ddfcbd4f8a847047b73f24a00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882382"
---
# <a name="settings"></a>Impostazioni

> Si applica a: Windows Admin Center

Windows Admin Center sono disponibili impostazioni a livello di utente e a livello di gateway. Una modifica a un utente a livello di impostazione influisce solo sul profilo dell'utente corrente, mentre una modifica a un'impostazione a livello di gateway influisce su tutti gli utenti in tale gateway Windows Admin Center.

## <a name="user-settings"></a>Impostazioni utente

Impostazioni a livello di utente sono le seguenti sezioni:

- Account
- Lingua/area geografica
- Suggerimenti

Nel **Account** scheda, gli utenti possono esaminare le credenziali sono usati per eseguire l'autenticazione di Windows Admin Center. Se Azure AD è configurato per il provider di identità, l'utente può disconnettersi dal proprio account Azure AD in questa scheda.

Nel **lingua/area geografica** scheda, gli utenti possono modificare i formati di lingua e area geografica visualizzati da Windows Admin Center.

Nel **suggerimenti** scheda, gli utenti possono attivare o disattivare i suggerimenti sulle nuove funzionalità e servizi di Azure.

### <a name="dark-theme"></a>Tema scuro

> Si applica a: Anteprima di Windows Admin Center

In fase di anteprima Windows Admin Center, è possibile sbloccare un ulteriore **personalizzazione** sezione, che contiene l'opzione per attivare/disattivare per un tema scuro di interfaccia utente. Per abilitare la **personalizzazione** , quindi immettere ```msft.sme.shell.personalization``` come una chiave di esperimento.

>[!IMPORTANT]
> Tema scuro è in corso, eseguire non segnala bug in questo momento.

## <a name="gateway-settings"></a>Impostazioni del gateway

Impostazioni a livello di gateway sono le seguenti sezioni:

- Extensions
- Accesso
- Azure

Solo gli amministratori di gateway sono in grado di visualizzare e modificare queste impostazioni. Le modifiche apportate a queste impostazioni modificare la configurazione del gateway e influiscono su tutti gli utenti del gateway Windows Admin Center.

Nel **estensioni** scheda, gli amministratori possono installare, disinstallare o aggiornare le estensioni di gateway. [Altre informazioni sulle estensioni.](using-extensions.md)

Il **accesso** scheda consente agli amministratori di configurare il gateway di Windows Admin Center, nonché provider di identità usato per autenticare gli utenti, chi può accedere. [Altre informazioni sul controllo dell'accesso al gateway.](user-access-control.md)

Dal **Azure** scheda, gli amministratori possono registrare il gateway con Azure per consentire [funzionalità di integrazione di Azure](azure-integration.md) in Windows Admin Center.