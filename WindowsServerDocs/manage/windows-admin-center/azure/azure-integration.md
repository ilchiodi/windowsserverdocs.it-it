---
title: Configurazione dell'integrazione di Azure
description: Configurazione dell'interfaccia di amministrazione di Windows per integrazione di Azure (Project Honolulu). Connessione del gateway dell'interfaccia di amministrazione di Windows ad Azure.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 448a8fb3e4340752b673b06f86d5d49211b6b147
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357364"
---
# <a name="configuring-azure-integration"></a>Configurazione dell'integrazione di Azure

>Si applica a: Windows Admin Center, Windows Admin Center Preview

L'interfaccia di amministrazione di Windows supporta diverse funzionalità facoltative che si integrano con i servizi di Azure. [Informazioni sulle opzioni di integrazione di Azure disponibili nell'interfaccia di amministrazione di Windows.](../plan/azure-integration-options.md)

Per consentire al gateway dell'interfaccia di amministrazione di Windows di comunicare con Azure per sfruttare l'autenticazione Azure Active Directory per l'accesso al gateway o per creare risorse di Azure per conto dell'utente, ad esempio per proteggere le macchine virtuali gestite nell'interfaccia di amministrazione di Windows usando il sito di Azure Ripristino), è necessario prima registrare il gateway dell'interfaccia di amministrazione di Windows con Azure. Questa operazione deve essere eseguita una sola volta per il gateway dell'interfaccia di amministrazione di Windows. l'impostazione viene mantenuta quando si aggiorna il gateway a una versione più recente.

## <a name="register-your-gateway-with-azure"></a>Registrare il gateway con Azure

La prima volta che si tenta di usare una funzionalità di integrazione di Azure nell'interfaccia di amministrazione di Windows, verrà richiesto di registrare il gateway in Azure. È anche possibile registrare il gateway passando alla scheda **Azure** in impostazioni dell'interfaccia di amministrazione di Windows.

La procedura guidata nel prodotto creerà un'app Azure AD nella directory, che consente all'interfaccia di amministrazione di Windows di comunicare con Azure. Per visualizzare l'app Azure AD creata automaticamente, passare alla scheda **Azure** delle impostazioni dell'interfaccia di amministrazione di Windows. Il collegamento ipertestuale **Visualizza in Azure** consente di visualizzare l'app Azure AD nel portale di Azure. 

L'app Azure AD creata viene usata per tutti i punti di integrazione di Azure nell'interfaccia di amministrazione di Windows, inclusa l' [autenticazione Azure ad per il gateway](../configure/user-access-control.md#azure-active-directory).