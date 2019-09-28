---
title: Installare la versione Server Core
description: Come ottenere e ed eseguire l'installazione dei componenti di base del server su Windows Server 2019, Windows Server 2016 o Windows Server (Canale semestrale).
ms.prod: windows-server
ms.date: 05/21/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: e6264a59a837003e49e82529750cfb153cc37b92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360344"
---
# <a name="install-server-core"></a>Installare la versione Server Core

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)
  
Quando installi Windows Server per la prima volta, hai a disposizione le opzioni di installazione seguenti:

>[!NOTE]
> Nell'elenco seguente le edizioni senza "Esperienza desktop" sono le opzioni di installazione dei componenti di base del server

-   Windows Server Standard
-   Windows Server Standard con Esperienza desktop
-   Windows Server Datacenter
-   Windows Server Datacenter con Esperienza desktop

Quando installi Windows Server (Canale semestrale), hai a disposizione le opzioni di installazione seguenti:

-   Windows Server Standard 
-   Windows Server Datacenter

Poiché l'opzione di installazione dei componenti di base del server riduce lo spazio richiesto sul disco e la superficie soggetta a possibili attacchi, ti consigliamo di scegliere questo tipo di installazione a meno che non ti servano gli elementi aggiuntivi dell'interfaccia utente e gli strumenti di gestione grafica inclusi nell'opzione Server con Esperienza desktop. Se credi di aver bisogno degli elementi aggiuntivi dell'interfaccia utente, vedi [Installare la versione Server con Esperienza desktop](Getting-Started-with-Server-with-Desktop-Experience.md). 

Con l'opzione di installazione dei componenti di base del server, l'interfaccia utente standard (Esperienza desktop) non viene installata e puoi gestire il server tramite la riga di comando, Windows PowerShell o metodi remoti.

>[!NOTE]
>
>Diversamente da alcune versioni precedenti di Windows Server, non puoi eseguire la conversione tra Server Core e la versione Server con Esperienza desktop dopo l'installazione. Se installi Server Core e successivamente decidi di usare la versione Server con Esperienza desktop, devi eseguire un'installazione pulita.

**Interfaccia utente:** prompt dei comandi

**Installare, configurare, disinstallare ruoli server in locale:** un prompt dei comandi con Windows PowerShell.

**Installare, configurare, disinstallare ruoli del server in remoto da un computer client Windows (o un server con Esperienza desktop installato):** con Server Manager, Strumenti di amministrazione remota del server, Windows PowerShell o Windows Admin Center.

>[!NOTE]
>
>Per gli Strumenti di amministrazione remota del server (RSAT), è necessario usare la versione di Windows 10.
>Microsoft Management Console non è disponibile in locale.

**Ruoli del server di esempio disponibili:**

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

Per informazioni sui ruoli non inclusi nell'installazione dei componenti di base del server, vedi [Ruoli, servizi ruolo e funzionalità non inclusi in Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>Installazione su Windows Server 2019 o Windows Server 2016

Per i passaggi di installazione generali per Windows Server (Long-Term Servicing Channel), vedi [Installazione e aggiornamento di Windows Server](installation-and-upgrade.md).

## <a name="installing-on-windows-server-semi-annual-channel"></a>Installazione su Windows Server (Canale semestrale)

I passaggi di installazione per Windows Server (Canale semestrale) sono gli stessi di quelli per l'installazione delle versioni precedenti di Windows Server (da un'immagine ISO), con le eccezioni seguenti:

- Non sono supportati aggiornamenti da versioni precedenti di Windows Server a Windows Server versione 1709. È necessario eseguire sempre un'installazione pulita.
   Questo significa che quando esegui setup.exe dal desktop di un computer Windows, l'esperienza di installazione non permette l'opzione di aggiornamento (l'opzione è disattivata).
- Non esiste alcuna versione di valutazione per Windows Server (Canale semestrale).
- Non esiste alcuna versione OEM o definitiva. Windows Server (Canale semestrale) può essere concesso in licenza solo tramite Software Assurance o programmi fedeltà.

Per altre informazioni su Canale semestrale, vedi [Confronto tra canali di manutenzione](../get-started-19/servicing-channels-19.md).

Per informazioni sulle novità di Windows Server (Canale semestrale), vedi [Novità di Windows Server](whats-new-in-windows-server.md)
