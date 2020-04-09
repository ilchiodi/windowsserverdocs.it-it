---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Pianificazione del posizionamento del controller di dominio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 784e26a1762e8950b8f658ea32968a0d170df13d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822184"
---
# <a name="planning-domain-controller-placement"></a>Pianificazione del posizionamento del controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver raccolto tutte le informazioni di rete che verranno usate per progettare la topologia del sito, pianificare dove si desidera posizionare i controller di dominio, inclusi i controller di dominio radice della foresta, i controller di dominio regionali, i titolari del ruolo master operazioni e i server di catalogo globale.  
  
In Windows Server 2008 è inoltre possibile sfruttare i vantaggi dei controller di dominio di sola lettura (RODC). Un controller di dominio di sola lettura è un nuovo tipo di controller di dominio che ospita partizioni di sola lettura del database di Active Directory. Ad eccezione delle password degli account, un controller di dominio di sola lettura include tutti gli oggetti e gli attributi Active Directory contenuti in un controller di dominio scrivibile. Tuttavia, non è possibile apportare modifiche al database archiviato nel RODC. Le modifiche devono essere apportate in un controller di dominio scrivibile e quindi replicate nel controller di dominio di sola lettura.  
  
Un controller di dominio di sola lettura è progettato principalmente per essere distribuito in ambienti remoti o succursali, che in genere hanno un numero relativamente basso di utenti, una scarsa sicurezza fisica, una larghezza di banda di rete relativamente bassa a un sito hub e il personale con una conoscenza limitata di Information Technology (IT). La distribuzione di RODC comporta una maggiore sicurezza e un accesso più efficiente alle risorse di rete. Per ulteriori informazioni sulle funzionalità di RODC, vedere Servizi di dominio Active Directory: controller di dominio di sola lettura ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Per informazioni su come distribuire un controller di dominio di sola lettura, vedere la guida dettagliata per i controller di dominio di sola lettura ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> In questa guida non viene illustrato come determinare il numero appropriato di controller di dominio e i requisiti hardware del controller di dominio per ogni dominio rappresentato in ogni sito.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Pianificazione del posizionamento del controller di dominio radice della foresta](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Pianificazione del posizionamento del controller di dominio regionale](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Pianificazione del posizionamento del server di catalogo globale](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Pianificazione del posizionamento del ruolo master operazioni](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


