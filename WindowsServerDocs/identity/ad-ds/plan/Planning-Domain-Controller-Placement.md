---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Pianificazione del posizionamento del controller di dominio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 68406d4973dd585bf0a98562c987c1b1512c095c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883362"
---
# <a name="planning-domain-controller-placement"></a>Pianificazione del posizionamento del controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver raccolto tutte le informazioni di rete che verranno usate per progettare la topologia del sito, pianificare in cui si desidera posizionare i controller di dominio, inclusi controller di dominio radice foresta, i controller di dominio regionale, titolari ruolo di master operazioni, e server di catalogo globale.  
  
In Windows Server 2008, è anche possibile sfruttare i vantaggi dei controller di dominio di sola lettura (RODC). Un RODC è un nuovo tipo di controller di dominio che ospita le partizioni di sola lettura del database di Active Directory. Fatta eccezione per le password degli account, un RODC contiene tutti gli oggetti Active Directory e attributi che contiene un controller di dominio scrivibile. Tuttavia, è Impossibile apportare modifiche al database che viene archiviato nel RODC. Le modifiche devono essere eseguite in un controller di dominio scrivibile e successivamente replicate nel RODC.  
  
Un RODC è progettato principalmente per essere distribuito in remoti o succursali, che in genere hanno un numero relativamente basso, sicurezza fisica non ottimale, larghezza di banda relativamente ridotte per un sito hub e il personale con una conoscenza limitata delle informazioni Technology (IT). Distribuzione RODC risultati una maggiore sicurezza e l'accesso più efficiente per le risorse di rete. Per altre informazioni sulle funzionalità di controller di dominio, vedere Active Directory Domain Services: I controller di dominio di sola lettura ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Per informazioni su come distribuire un controller di dominio, vedere la Guida dettagliata per i controller di dominio di sola lettura ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Questa Guida non viene illustrato come si determina il numero adeguato di controller di dominio e i requisiti hardware di controller di dominio per ogni dominio che viene rappresentato in ogni sito.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Pianificazione della selezione del Controller di dominio radice foresta](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Pianificazione della selezione del Controller di dominio regionale](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Pianificazione del posizionamento di Server di catalogo globale](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Pianificazione del posizionamento del ruolo Master operazioni](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


