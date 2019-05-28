---
title: Comandi di Git Bash comuni per l'uso con GitHub
description: Elenco di alcuni comandi usati più di frequente in Git Bash quando si lavora con GitHub.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 210acaf2b7911892bcfd81b6bbe1975f141308a1
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461696"
---
# <a name="common-git-bash-commands"></a>Comandi di Git Bash comuni

Questi sono alcuni dei comandi utilizzate più di frequente in Git Bash, in base a quando li si utilizzerà la creazione di contenuto e la modifica processo.

## <a name="master-branch-related"></a>Correlati al ramo master

È sempre necessario utilizzare schema come base per qualsiasi nuovo ramo.

| Comando | Descrizione |
|---------|-------------|
| `git checkout master` | Passare allo schema da qualsiasi altro ramo |
| `git pull upstream master` | Aggiornare la copia locale del database master dal repository di produzione |

## <a name="branch-related"></a>Rami correlati

| Comando | Descrizione |
|---------|-------------|
| `git branch` | Visualizzare i rami esistenti |
| `git checkout -B <name-of-branch>` | Creare un nuovo ramo |
| `git checkout <name-of-branch>` | Modificare in un altro ramo |
| `git status` | Controllare cosa sta succedendo nel ramo |
| `git branch -D <name-of-branch>` | Eliminare un ramo esistente (rendendo che non sia in esso) |

## <a name="check-in-related-done-as-a-group-in-order"></a>Check-in-correlati (eseguita come un gruppo, in ordine)

| Comando | Descrizione |
|---------|-------------|
| `git add --all` | Dopo aver salvato il lavoro, aggiungerlo a un ramo |
| `git commit -m “public comment, including quotes”` | Eseguire il commit delle modifiche nel ramo |
| `git pull upstream master` | Aggiornare la copia locale del database master dal repository di produzione |
| `git push origin <name-of-branch>` | Eseguire il push per la versione remota del ramo locale |