---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Pianificazione della posizione del controller di dominio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: bb68a3551b362f0beb5d42a92a7c18d0913e47bc
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624089"
---
# <a name="planning-domain-controller-placement"></a>Pianificazione della posizione del controller di dominio

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver raccolto tutte le informazioni di rete che verranno usate per progettare la topologia del sito, pianificare dove si desidera posizionare i controller di dominio, inclusi i controller di dominio radice della foresta, i controller di dominio regionali, i titolari del ruolo master operazioni e i server di catalogo globale.

In Windows Server 2008 è inoltre possibile sfruttare i vantaggi dei controller di dominio di sola lettura (RODC). Un controller di dominio di sola lettura è un nuovo tipo di controller di dominio che ospita partizioni di sola lettura del database di Active Directory. Ad eccezione delle password degli account, un controller di dominio di sola lettura include tutti gli oggetti e gli attributi Active Directory contenuti in un controller di dominio scrivibile. Tuttavia, non è possibile apportare modifiche al database archiviato nel RODC. Le modifiche devono essere apportate in un controller di dominio scrivibile e quindi replicate nel controller di dominio di sola lettura.

Un controller di dominio di sola lettura è progettato principalmente per essere distribuito in ambienti remoti o succursali, che in genere hanno un numero relativamente basso di utenti, una scarsa sicurezza fisica, una larghezza di banda di rete relativamente bassa a un sito hub e il personale con una conoscenza limitata di Information Technology (IT). La distribuzione di RODC comporta una maggiore sicurezza e un accesso più efficiente alle risorse di rete. Per ulteriori informazioni sulle funzionalità di RODC, vedere [servizi di dominio Active Directory: controller di dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732801(v=ws.10))di sola lettura. Per informazioni su come distribuire un controller di dominio di sola lettura, vedere la [Guida dettagliata ai controller di dominio di sola lettura](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772234(v=ws.10))

> [!NOTE]
> In questa guida non viene illustrato come determinare il numero appropriato di controller di dominio e i requisiti hardware del controller di dominio per ogni dominio rappresentato in ogni sito.

## <a name="in-this-section"></a>Contenuto della sezione

- [Pianificazione del posizionamento del controller di dominio radice della foresta](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)

- [Pianificazione del posizionamento del controller di dominio regionale](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)

- [Pianificazione del posizionamento del server di catalogo globale](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)

- [Pianificazione del posizionamento del ruolo master operazioni](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)

