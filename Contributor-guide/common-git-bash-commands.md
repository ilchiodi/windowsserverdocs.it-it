---
title: Comandi comuni di git bash per l'uso con GitHub
description: Un elenco di alcuni dei comandi usati più di frequente in git bash quando si lavora con GitHub.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 4ce5d4d8ce382e9ba421c20595715ec473cca241
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865001"
---
# <a name="common-git-bash-commands"></a>Comandi Git bash comuni

Questi sono alcuni dei comandi più usati in git bash, in base a quando verranno usati nel processo di creazione e modifica del contenuto.

## <a name="master-branch-related"></a>Correlato al ramo master

È sempre necessario usare master come base per qualsiasi nuovo ramo.

| Comando | Descrizione |
|---------|-------------|
| `git checkout master` | Passa a master da qualsiasi altro ramo |
| `git pull upstream master` | Aggiornare la copia locale del master dal repository di produzione |

## <a name="branch-related"></a>Correlate al ramo

| Comando | Descrizione |
|---------|-------------|
| `git branch` | Visualizza i rami esistenti |
| `git checkout -B <name-of-branch>` | Creare un nuovo ramo |
| `git checkout <name-of-branch>` | Passa a un altro branch |
| `git status` | Controllare gli elementi in corso nel ramo |
| `git branch -D <name-of-branch>` | Eliminare un ramo esistente (assicurandosi di non essere al suo interno) |

## <a name="check-in-related-done-as-a-group-in-order"></a>Relativo all'archiviazione (operazione eseguita come gruppo, in ordine)

| Comando | Descrizione |
|---------|-------------|
| `git add --all` | Dopo aver salvato il lavoro, aggiungerlo a un ramo |
| `git commit -m “public comment, including quotes”` | Eseguire il commit delle modifiche nel ramo |
| `git pull upstream master` | Aggiornare la copia locale del master dal repository di produzione |
| `git push origin <name-of-branch>` | Eseguire il push alla versione remota del Branch locale |