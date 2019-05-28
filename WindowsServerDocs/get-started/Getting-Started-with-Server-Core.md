---
title: Installare la versione Server Core
description: Come ottenere e installare un'installazione Server Core in Windows Server (canale semestrale), Windows Server 2016 o Windows Server 2019.
ms.prod: windows-server-threshold
ms.date: 05/21/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 6f685ce29088b56bb243d21315787ab90e6863a4
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976725"
---
# <a name="install-server-core"></a>Installare la versione Server Core

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)
  
Quando si installa Windows Server per la prima volta, sono disponibili le opzioni di installazione seguenti:

>[!NOTE]
> Nell'elenco seguente, le edizioni senza "Esperienza Desktop" sono le opzioni di installazione Server Core

-   Windows Server Standard
-   Windows Server Standard con Esperienza desktop
-   Windows Server Datacenter
-   Windows Server Datacenter con Esperienza desktop

Quando si installa Windows Server (canale semestrale), sono disponibili le opzioni di installazione seguenti:

-   Windows Server Standard 
-   Windows Server Datacenter

Poiché l'opzione Server Core riduce lo spazio richiesto sul disco e la superficie soggetta a possibili attacchi e, in particolare, i requisiti di manutenzione, è consigliabile scegliere questo tipo di installazione a meno che non occorrano elementi aggiuntivi dell'interfaccia utente e strumenti di gestione grafica inclusi nell'opzione Server con Esperienza desktop. Se si ritiene necessario usare gli elementi aggiuntivi dell'interfaccia utente, vedere [Installare Server con Esperienza desktop](Getting-Started-with-Server-with-Desktop-Experience.md). 

Con l'opzione Server Core, l'interfaccia utente standard (Esperienza Desktop) non viene installata. Puoi gestire il server usando la riga di comando, Windows PowerShell o metodi remoti.

>[!NOTE]
>
>Diversamente da alcune versioni precedenti di Windows Server, non puoi eseguire la conversione tra Server Core e la versione Server con Esperienza desktop dopo l'installazione. Se installi Server Core e successivamente decidi di usare la versione Server con Esperienza desktop, devi eseguire un'installazione pulita.

**Interfaccia utente:** prompt dei comandi

**Installare, configurare, disinstallare ruoli server in locale:** un prompt dei comandi con Windows PowerShell.

**Installare, configurare, disinstallare ruoli server in modalità remota da un computer client Windows (o un server con esperienza Desktop installato):** con Server Manager, strumenti di amministrazione remota Server (RSAT), Windows PowerShell o Windows Admin Center .

>[!NOTE]
>
>Per gli Strumenti di amministrazione remota del server (RSAT), è necessario usare la versione di Windows 10.
>Microsoft Management Console non è disponibile localmente.

**Ruoli server di esempio disponibili:**

- Servizi certificati Active Directory
- Servizi di dominio di Active Directory
- Server DHCP
- Server DNS
- Servizi file (incluso Gestione risorse file server)
- Active Directory Lightweight Directory Services
- Hyper-V
- Servizi di stampa e digitalizzazione
- Servizi flussi multimediali
- Server Web (incluso un sottoinsieme di ASP.NET)
- Server di aggiornamento di Windows Server
- Server AD RMS (Active Directory Rights Management Services)
- Server di Routing e Accesso remoto e i ruoli secondari seguenti:
   - Gestore connessione Servizi Desktop remoto
   - Gestione licenze
   - Virtualizzazione
   - Servizi di attivazione contratti multilicenza

Per i ruoli non inclusi in Server Core, vedere [ruoli, servizi ruolo e funzionalità non in Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>Installazione in Windows Server 2016 o Windows Server 2019

Per passaggi di installazione generale e le opzioni per Windows Server (canale di manutenzione di lungo termine), vedere [installazione di Windows Server e di aggiornamento](installation-and-upgrade.md).

## <a name="installing-on-windows-server-semi-annual-channel"></a>Installazione in Windows Server (canale semestrale)

Passaggi di installazione per Windows Server (canale semestrale) sono le stesse di installazione di versioni precedenti di Windows Server (da una. Immagine ISO), con le eccezioni seguenti:

- Non sono supportati aggiornamenti da versioni precedenti di Windows Server a Windows Server, versione 1709. Una nuova installazione è sempre obbligatoria.
   Ciò significa che quando si esegue setup.exe dal desktop di un computer Windows, l'esperienza di installazione non supporta l'opzione di aggiornamento (disabilitato).
- Nessuna versione di valutazione per Windows Server (canale semestrale)
- Non esiste alcun OEM o una versione definitiva. Windows Server (canale semestrale) possono solo avere la licenza tramite Software Assurance o la fedeltà dei programmi.

Per altre informazioni su canale semestrale, vedere [confronto dei canali di manutenzione](../get-started-19/servicing-channels-19.md).

Per vedere quali sono le novità nel canale semestrale di Windows Server, vedere [What ' s New in Windows Server](whats-new-in-windows-server.md)
