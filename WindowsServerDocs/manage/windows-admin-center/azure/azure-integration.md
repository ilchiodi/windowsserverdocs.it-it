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
ms.openlocfilehash: d8758342752ac71c5b700682d4c0f4317dc4cb4e
ms.sourcegitcommit: e817a130c2ed9caaddd1def1b2edac0c798a6aa2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2019
ms.locfileid: "74945216"
---
# <a name="configuring-azure-integration"></a>Configurazione dell'integrazione di Azure

>Si applica a: Windows Admin Center, Windows Admin Center Preview

L'interfaccia di amministrazione di Windows supporta diverse funzionalità facoltative che si integrano con i servizi di Azure. [Informazioni sulle opzioni di integrazione di Azure disponibili nell'interfaccia di amministrazione di Windows.](../plan/azure-integration-options.md)

Per consentire al gateway dell'interfaccia di amministrazione di Windows di comunicare con Azure per sfruttare l'autenticazione Azure Active Directory per l'accesso al gateway o per creare risorse di Azure per conto dell'utente, ad esempio per proteggere le macchine virtuali gestite nell'interfaccia di amministrazione di Windows usando il sito di Azure Ripristino), è necessario prima registrare il gateway dell'interfaccia di amministrazione di Windows con Azure. Questa operazione deve essere eseguita una sola volta per il gateway dell'interfaccia di amministrazione di Windows. l'impostazione viene mantenuta quando si aggiorna il gateway a una versione più recente.

## <a name="register-your-gateway-with-azure"></a>Registrare il gateway con Azure

La prima volta che si tenta di usare una funzionalità di integrazione di Azure nell'interfaccia di amministrazione di Windows, verrà richiesto di registrare il gateway in Azure. È anche possibile registrare il gateway passando alla scheda **Azure** in impostazioni dell'interfaccia di amministrazione di Windows. Si noti che solo gli amministratori del gateway Windows Admin Center possono registrare il gateway dell'interfaccia di amministrazione di Windows con Azure. [Altre informazioni sulle autorizzazioni utente e amministratore di Windows Admin Center](../configure/user-access-control.md#gateway-access-role-definitions).

La procedura guidata nel prodotto creerà un'app Azure AD nella directory, che consente all'interfaccia di amministrazione di Windows di comunicare con Azure. Per visualizzare l'app Azure AD creata automaticamente, passare alla scheda **Azure** delle impostazioni dell'interfaccia di amministrazione di Windows. Il collegamento ipertestuale **Visualizza in Azure** consente di visualizzare l'app Azure AD nel portale di Azure. 

L'app Azure AD creata viene usata per tutti i punti di integrazione di Azure nell'interfaccia di amministrazione di Windows, inclusa l' [autenticazione Azure ad per il gateway](../configure/user-access-control.md#azure-active-directory). Centro di amministrazione di Windows configura automaticamente le autorizzazioni necessarie per creare e gestire le risorse di Azure per conto dell'utente:

- Azure Active Directory Graph
    - Directory.AccessAsUser.All
    - User.Read
- Gestione servizi di Azure
    - user_impersonation

### <a name="manual-azure-ad-app-configuration"></a>Configurazione manuale dell'app Azure AD

Se si vuole configurare manualmente un'app Azure AD, anziché usare l'app Azure AD creata automaticamente dall'interfaccia di amministrazione di Windows durante il processo di registrazione del gateway, è necessario eseguire le operazioni seguenti.

1. Concedere all'app Azure AD le autorizzazioni API richieste sopra elencate. È possibile eseguire questa operazione passando all'app Azure AD nel portale di Azure. Passare alla portale di Azure > **Azure Active Directory** ** > registrazioni app > selezionare** l'app Azure ad che si vuole usare. Quindi alla scheda **autorizzazioni API** e aggiungere le autorizzazioni API elencate in precedenza.
2. Aggiungere l'URL del gateway dell'interfaccia di amministrazione di Windows agli URL di risposta (noti anche come URI di reindirizzamento). Passare all'app Azure AD, quindi passare a **manifest**. Trovare la chiave "replyUrlsWithType" nel manifesto. All'interno della chiave aggiungere un oggetto contenente due chiavi: "URL" e "Type". La chiave "URL" deve avere un valore dell'URL del gateway dell'interfaccia di amministrazione di Windows, aggiungendo un carattere jolly alla fine. Il valore della chiave "Type" deve essere "Web". Ad esempio:

    ```json
    "replyUrlsWithType": [
            {
                    "url": "http://localhost:6516/*",
                    "type": "Web"
            }
    ],
    ```
