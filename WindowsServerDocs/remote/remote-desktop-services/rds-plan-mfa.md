---
title: Servizi Desktop remoto - Autenticazione a più fattori
description: Informazioni di pianificazione per l'uso dell'autenticazione a più fattori con Servizi desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: c46ad24c62510b4a100a89b5c10a8f52c1a66151
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857354"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Servizi Desktop remoto - Autenticazione a più fattori

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Sfrutta la potenza di Active Directory con l'autenticazione a più fattori per proteggere le risorse aziendali con un elevato livello di sicurezza.

Per gli utenti finali che si connettono ai rispettivi desktop e applicazioni, l'esperienza è analoga a quella che vivono già quando eseguono una seconda operazione di autenticazione per connettersi alla risorsa desiderata:
- Avviano un desktop o RemoteApp da un file RDP o tramite un'applicazione client Desktop remoto
- Al momento della connessione al Gateway Desktop remoto per un accesso remoto sicuro, ricevono un SMS o test con autenticazione a più fattori tramite l'applicazione mobile
- Si autenticano correttamente e vengono connessi alla risorsa

Per altri dettagli sul processo di configurazione, vedi [Integrare l'infrastruttura del Gateway Desktop remoto usando l'estensione Server dei criteri di rete e Azure AD](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway).
