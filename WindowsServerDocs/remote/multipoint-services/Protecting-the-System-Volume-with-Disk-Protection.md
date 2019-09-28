---
title: Protezione del volume di sistema con Protezione disco
description: Fornisce informazioni sulla protezione del disco per servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18694665-ed65-4d84-8505-f460cf3df907
author: evaseydl
manager: scotman
ms.author: evas
ms.openlocfilehash: 27bcce68cfe5856e987f5ad93460abd12217c26b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389515"
---
# <a name="protecting-the-system-volume-with-disk-protection"></a>Protezione del volume di sistema con Protezione disco
MultiPoint Services consente di cancellare immediatamente le modifiche apportate al volume di sistema ogni volta che il computer viene avviato. Se si Abilita la funzionalità di protezione del disco, le eventuali modifiche apportate all'unità, ad esempio il danneggiamento della configurazione o l'introduzione di malware, verranno annullate al successivo riavvio del computer. Si tratta di una funzionalità utile per gli amministratori che vogliono assicurare che ogni volta venga caricata un'immagine software "valida" o "dorata" nota. Gli aggiornamenti automatici o l'applicazione di patch software possono essere pianificati, ad esempio, nel corso della notte. La considerazione della pianificazione è se si desidera che gli utenti finali possano apportare modifiche, ad esempio l'installazione di software, da Internet. Se questa funzionalità è abilitata, se si desidera che gli utenti siano in grado di archiviare i file, è necessario che la condivisione file si trovi all'esterno del volume di sistema.  
  
