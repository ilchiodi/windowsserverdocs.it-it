---
title: Configurare i limiti delle notifiche
description: Questo articolo descrive come aggiungere limiti di tempo ai vari tipi di notifica
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: dba5b3b3c8b651935ec3c69695583d04087b7f2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826312"
---
# <a name="configure-notification-limits"></a>Configurare i limiti delle notifiche

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Per ridurre il numero di notifiche accumulate a causa del ripetuto superamento del limite di quota o del tentativo di salvare un file non autorizzato, Gestione risorse file server applica limiti di tempo ai seguenti tipi di notifica:

-   Posta elettronica
-   Registro eventi
-   Comando
-   Report

Ogni limite specifica un periodo di tempo prima che un'altra notifica configurata dello stesso tipo venga generata per un problema identico.

Un limite di 60 minuti predefinito è impostato per ogni tipo di notifica, ma è possibile modificarlo. Il limite si applica a tutte le notifiche di un determinato tipo, indipendentemente dal fatto che siano generate da limiti di quota o eventi di screening dei file.

## <a name="to-specify-a-standard-notification-limit-for-each-notification-type"></a>Per specificare un limite di notifica standard per ogni tipo di notifica

1.  Nell'albero della console, fare clic con il pulsante destro del mouse su **Gestione risorse file server**, quindi scegliere **Configura opzioni**. Verrà visualizzata la finestra di dialogo **Gestione risorse file server**.

2.  Nella scheda **Limiti notifica**, immettere un valore in minuti per ogni tipo di notifica che viene visualizzato.

3.  Fare clic su **OK**.

> [!Note]
> Per personalizzare i limiti di tempo associati alle notifiche per una quota o screening dei file specifici, è possibile utilizzare gli strumenti da riga di comando di Gestione risorse file server **Dirquota.exe** e **Filescrn.exe**, oppure utilizzare i cmdlet [File Server Resource Manager](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager).

## <a name="see-also"></a>Vedere anche

-   [Opzioni di gestione risorse di impostazione File Server](setting-file-server-resource-manager-options.md)
-   [Strumenti da riga di comando](command-line-tools.md)