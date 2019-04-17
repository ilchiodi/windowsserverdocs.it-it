---
title: Installare Server Core
description: Come ottenere e installare un'installazione Server Core in Windows Server 2019, Windows Server 2016 o Windows Server (canale semestrale).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 1/04/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: d99cd0b028d08d5c3247541ce3a868676b60693d
ms.sourcegitcommit: 7fc7271745e40f110c54918b55624cadd0d7ff98
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/04/2019
ms.locfileid: "8991798"
---
# Installare Server Core

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)
  
Quando si installa Windows Server per la prima volta, sono disponibili le seguenti opzioni di installazione:

>[!NOTE]
> Nell'elenco seguente, le edizioni senza "Esperienza Desktop" sono le opzioni di installazione Server Core

-   Windows Server Standard
-   Windows Server Standard con Esperienza desktop
-   Windows Server Datacenter
-   Windows Server Datacenter con Esperienza desktop

Quando si installa Windows Server (canale semestrale), incluse le versioni 1709 e 1803 1809, sono disponibili le seguenti opzioni di installazione:

-   Windows Server Standard 
-   Windows Server Datacenter

Poiché l'opzione Server Core riduce lo spazio richiesto sul disco e la superficie soggetta a possibili attacchi e, in particolare, i requisiti di manutenzione, è consigliabile scegliere questo tipo di installazione a meno che non occorrano elementi aggiuntivi dell'interfaccia utente e strumenti di gestione grafica inclusi nell'opzione Server con Esperienza desktop. Se si ritiene necessario usare gli elementi aggiuntivi dell'interfaccia utente, vedere [Installare Server con Esperienza desktop](Getting-Started-with-Server-with-Desktop-Experience.md). 

Con l'opzione Server Core, l'interfaccia utente standard (Esperienza Desktop) non viene installata. Puoi gestire il server usando la riga di comando, Windows PowerShell o metodi remoti.

>[!NOTE]
>
>Diversamente da alcune versioni precedenti di Windows Server, non puoi eseguire la conversione tra Server Core e la versione Server con Esperienza desktop dopo l'installazione. Se installi Server Core e successivamente decidi di usare la versione Server con Esperienza desktop, devi eseguire un'installazione pulita.

**Interfaccia utente:** prompt dei comandi

**Installare, configurare, disinstallare ruoli server in locale:** un prompt dei comandi con Windows PowerShell.

**Installare, configurare, disinstallare ruoli server in modalità remota da un computer client Windows (o un server con esperienza Desktop installata):** con Server Manager, strumenti di amministrazione remota Server (RSAT), Windows PowerShell o Windows Admin Center.

>[!NOTE]
>
>Per Strumenti di amministrazione remota del server è necessario usare la versione Windows 10.
>Microsoft Management Console non è disponibile localmente.

**Ruoli server di esempio disponibili:**

- Servizi certificati Active Directory
- Active Directory Domain Services
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

Per i ruoli non inclusi nel Server Core, vedi [ruoli, servizi ruolo e funzionalità non in Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## L'installazione in Windows Server 2019 o Windows Server 2016

Per i passaggi generali di installazione e le opzioni per Windows Server (Long Term Servicing Channel), vedi [l'aggiornamento e installazione di Windows Server](installation-and-upgrade.md).

## Installazione su Windows Server (canale semestrale)

Passaggi di installazione per Windows Server (canale semestrale) sono gli stessi come l'installazione delle versioni precedenti di Windows Server (da un. Immagine ISO), con le eccezioni seguenti:
- Non sono supportati aggiornamenti da versioni precedenti di Windows Server a Windows Server, versione 1709. Una nuova installazione è sempre obbligatoria.
   Ciò significa che quando si esegue setup.exe dal desktop di un computer Windows, l'esperienza di installazione non consente l'opzione di aggiornamento (è disponibile).
- Non esiste alcuna versione di valutazione per Windows Server (canale semestrale)
- Non esiste alcun OEM o una versione definitiva. Windows Server (canale semestrale) può essere concesso in licenza solo mediante programmi Software Assurance o fedeltà.

Per ottenere Windows Server versione 1709, vedere [Introduzione a Windows Server, versione 1709](get-started-with-1709.md).

Per ottenere Windows Server versione 1803, vedere [Introduzione a Windows Server, versione 1803](get-started-with-1803.md).

Per novità in Windows Server, versione 1809, vedi [Novità in Windows Server versione 1809](whats-new-in-windows-server-1809.md)
