---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Pianificazione del posizionamento di Controller di dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c6d9ca064699f95390e5b95863a6871ea2dc9d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="planning-domain-controller-placement"></a>Pianificazione del posizionamento di Controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver raccolto tutte le informazioni di rete che verranno utilizzate per progettare la topologia del sito, pianificare in cui si desidera posizionare i controller di dominio, inclusi controller di dominio radice foresta, i controller di dominio regionale di master operazioni titolari del ruolo e i server di catalogo globale.  
  
In Windows Server 2008, è possibile usufruire dei controller di dominio di sola lettura (RODC). Un RODC è un nuovo tipo di controller di dominio che ospita le partizioni di sola lettura del database di Active Directory. Fatta eccezione per le password degli account, un RODC contiene tutti gli oggetti di Active Directory e gli attributi che contiene un controller di dominio scrivibile. Tuttavia, è Impossibile apportare modifiche al database archiviati nel RODC. Modifiche devono essere eseguite in un controller di dominio scrivibile e quindi replicate nel RODC.  
  
Un RODC è progettato principalmente per essere distribuiti in remoto o filiali, che hanno in genere un numero relativamente ridotto utenti, la sicurezza fisica, della larghezza di banda di rete relativamente insufficienti per un sito hub e il personale con una conoscenza limitata di informazioni reparti IT. Distribuzione di risultati di lettura in migliorare la sicurezza e un accesso più efficiente alle risorse di rete. Per ulteriori informazioni sulle funzionalità di controller di dominio Active Directory vedere: Read-Only i controller di dominio ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Per informazioni su come distribuire un controller di dominio, vedere il Step-by-Step Guida per Read-Only controller di dominio ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Questa Guida non spiega come determinare il numero adeguato di controller di dominio e i requisiti hardware di controller di dominio per ogni dominio che è rappresentato in ogni sito.  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Pianificazione posizionamento del Controller di dominio radice foresta](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Pianificazione posizionamento del Controller di dominio regionale](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Pianificazione del posizionamento di Server di catalogo globale](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Pianificazione del posizionamento del ruolo di Master operazioni](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


