---
title: Configurazione dell'integrazione di Azure
description: Configurazione di Azure integrazione di Windows Admin Center (Project Honolulu). Connessione il gateway Windows Admin Center di Azure.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 38b973680463cdebc1b3168e447abfcf1caba6b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296995"
---
# Configurazione dell'integrazione di Azure

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center supporta diverse funzionalità facoltative che si integrano con servizi di Azure. [Scopri le opzioni di integrazione di Azure disponibili con Windows Admin Center.](../plan/azure-integration-options.md)

Per consentire il gateway Windows Admin Center comunicare con Azure sfruttare l'autenticazione di Azure Active Directory per l'accesso gateway o per creare le risorse di Azure per conto dell'utente (ad esempio, per proteggere le macchine virtuali gestite in Windows Admin Center con Azure Site Ripristino), dovrai prima registrare il gateway Windows Admin Center con Azure. È sufficiente eseguire questa operazione una volta per il gateway Windows Admin Center - l'impostazione viene mantenuta quando si aggiorna il gateway di una versione più recente.

## Registrare il gateway con Azure

La prima volta che si tenta di utilizzare una funzionalità di integrazione di Azure in Windows Admin Center, verrà richiesto di registrare il gateway di Azure. Puoi anche registrare il gateway, Vai alla scheda **Azure** nelle impostazioni di Windows Admin Center.

I passaggi di prodotto interattiva, crea un'app di Azure AD nella tua directory, che consente di Windows Admin Center comunicare con Azure. Per visualizzare l'app ad Azure AD che viene creato automaticamente, Vai alla scheda **Azure** delle impostazioni di Windows Admin Center. Il collegamento ipertestuale **visualizzazione in Azure** ti permette di visualizzare l'app ad Azure AD nel portale di Azure. 

L'app di Azure AD creato viene usato per tutti i punti di integrazione di Azure in Windows Admin Center, tra cui [l'autenticazione di Azure AD al gateway](../configure/user-access-control.md#azure-active-directory).