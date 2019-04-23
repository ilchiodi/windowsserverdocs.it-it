---
title: Pianificazione foglio di lavoro per la migrazione di servizi MultiPoint
description: Fornisce i fogli di lavoro di pianificazione per eseguire la migrazione ai servizi MultiPoint in Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: a9d9b62bced9be90c658b79338c6f4ef07710fc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880582"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>Pianificazione foglio di lavoro per la migrazione di servizi MultiPoint

>Si applica a: Windows Server 2016

Utilizzare elenchi e le tabelle seguenti per raccogliere le impostazioni che necessarie durante la migrazione di servizi MultiPoint.

## <a name="source-server-settings"></a>Impostazioni del server di origine

È possibile trovare le impostazioni del server sul **Home** scheda selezionare Gestione MultiPoint. Inserire un segno di spunta accanto a ciascuna impostazione in uso nel server di origine.

- Consentire a un account avere più sessioni.
- Consentire al computer di gestione remota.
- Consenti il monitoraggio dei desktop del computer.
- Avviare sempre in modalità console.
- Non visualizzare la notifica sulla privacy al primo accesso dell'utente.
- Assegnare un indirizzo IP univoco a ogni stazione.
- Consenti messaggistica immediata tra Dashboard MultiPoint e le sessioni utente in questo computer.
- Consenti orchestrazione di amministratore e le sessioni utente Dashboard MultiPoint.
- Consenti le stazioni da utilizzare per il rendering hardware GPU.

## <a name="managed-servers-and-computers"></a>I computer e i server gestiti

Registrare i nomi dei server gestiti e computer. È possibile trovare queste informazioni nel **Home** scheda selezionare Gestione MultiPoint.

| Computer | Il nome del computer |
|----------|---------------|
| 1        |               |
| 2        |               |
| 3        |               |
| 4        |               |
| 5        |               |
| 6        |               |
| 7        |               |
| 8        |               |
| 9        |               |
| 10       |               |


## <a name="stations"></a>Stazioni

Registrare le stazioni locali e le relative impostazioni. È possibile trovare queste informazioni nel **stazioni** scheda selezionare Gestione MultiPoint.

| #  | Nome stazione | Account utente l'accesso automatico | Orientamento dello schermo |
|----|--------------|-------------------------|---------------------|
| 1  |              |                         |                     |
| 2  |              |                         |                     |
| 3  |              |                         |                     |
| 4  |              |                         |                     |
| 5  |              |                         |                     |
| 6  |              |                         |                     |
| 7  |              |                         |                     |
| 8  |              |                         |                     |
| 9  |              |                         |                     |
| 10 |              |                         |                     |

## <a name="administrators-and-multipoint-dashboard-users"></a>Gli amministratori e utenti di Dashboard MultiPoint

Copiare i nomi utente per gli amministratori e utenti di Dashboard MultiPoint. È possibile trovare queste informazioni nel **utenti** scheda selezionare Gestione MultiPoint.

Administrators:

- nome utente:
- nome utente:
- nome utente:
- nome utente:
- nome utente:
- nome utente:

Utenti del dashboard:

- nome utente:
- nome utente:
- nome utente:
- nome utente:
- nome utente:

## <a name="vdi-template-and-virtual-desktops"></a>Modello di un'infrastruttura VDI e desktop virtuali

Registrare le informazioni sul modello di un'infrastruttura VDI e i nomi dei desktop virtuali nella distribuzione di MultiPoint Services. È possibile trovare queste informazioni nel **desktop virtuali** scheda selezionare Gestione MultiPoint.

**Percorso del modello VDI**: 

| # | Nome del desktop virtuale      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |