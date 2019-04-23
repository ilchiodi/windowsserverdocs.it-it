---
title: rdpsign
description: Informazioni su come firmare digitalmente un file RDP.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 648b179f5b2feb8a7585c815aee47804e3bf1532
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882272"
---
# <a name="rdpsign"></a>rdpsign

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di firmare digitalmente un file Remote Desktop Protocol (RDP).
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/sha1 \<hash>|Specifica l'identificazione personale, ovvero l'hash di Secure Hash Algorithm 1 (SHA1) del certificato di firma che è incluso nell'archivio certificati.|
|/q|Modalità non interattiva. Nessun output quando il comando ha esito positivo e un output minimo se il comando non riesce.|
|/v|modalità dettagliata. Visualizza tutti gli avvisi, messaggi e stato.|
|/l|Verifica i risultati di firma e di output senza effettivamente sostituire uno qualsiasi dei file di input.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   L'identificazione personale del certificato SHA1 deve rappresentare un server di pubblicazione di file con estensione RDP attendibili. Per ottenere l'identificazione personale del certificato, aprire lo snap-in certificati, fare doppio clic sul certificato che si desidera usare (nell'archivio certificati del computer locale o nell'archivio certificati personali), fare clic sui **dettagli** scheda e quindi il **campo** fare clic su **identificazione personale**.

    > [!NOTE]
    > Quando si copia l'identificazione personale per l'uso con lo strumento rdpsign.exe, è necessario rimuovere eventuali spazi.

-   È necessario specificare il file con estensione RDP (o file) di accedere usando il nome completo del file. I caratteri jolly non sono accettati.
-   I file di output con segno sovrascriverà i file di input.
-   Se qualsiasi file con estensione rdp non possono essere letti o scritto, lo strumento continuerà al file successivo se sono stati specificati più file.

## <a name="BKMK_examples"></a>Esempi
-   Per firmare un file con estensione rdp è denominato File1.rdp, passare alla cartella in cui è stato salvato il file con estensione rdp e quindi digitare il comando seguente:
    ```
    rdpsign /sha1 hash file1.rdp
    ```
    > [!NOTE]
    > Il *hash* valore rappresenta l'identificazione personale certificato SHA1, senza spazi.
-   Per verificare se la firma digitale avrà esito positivo per un file con estensione rdp senza effettivamente la firma del file, digitare quanto segue:
    ```
    rdpsign /sha1 hash /l file1.rdp
    ```
-   Per accedere più file con estensione rdp, separare i nomi dei file da spazi. Ad esempio, per accedere più file con estensione rdp che sono denominati File1.rdp File2.rdp e File3.rdp, digitare quanto segue:
    ```
    rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
    ```
## <a name="see-also"></a>Vedere anche
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)
