---
title: rdpsign
description: Informazioni su come firmare digitalmente un file RDP.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 406563a07d3760c2846c201410f3a7b8f1c2829b
ms.sourcegitcommit: b7f55949f166554614f581c9ddcef5a82fa00625
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72588052"
---
# <a name="rdpsign"></a>rdpsign

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di firmare digitalmente un file Remote Desktop Protocol (RDP).
per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|> hash \</SHA1|Specifica l'identificazione personale, ovvero l'hash Secure Hash Algorithm 1 (SHA1) del certificato di firma incluso nell'archivio certificati. Usato in Windows Server 2012 R2 e versioni precedenti.|
|> hash \</SHA256|Specifica l'identificazione personale, ovvero l'hash Secure Hash Algorithm 256 (SHA256) del certificato di firma incluso nell'archivio certificati. Sostituisce/SHA1 in Windows Server 2016 e versioni successive.|
|/q|Modalità non interattiva. Nessun output quando il comando ha esito positivo e output minimo se il comando ha esito negativo.|
|/v|modalità dettagliata. Visualizza tutti gli avvisi, i messaggi e lo stato.|
|/l|Verifica i risultati di firma e output senza sostituire effettivamente uno dei file di input.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
-   L'identificazione personale del certificato SHA1 o SHA256 deve rappresentare un server di pubblicazione trusted con estensione RDP. Per ottenere l'identificazione personale del certificato, aprire lo snap-in certificati, fare doppio clic sul certificato che si desidera utilizzare (nell'archivio certificati del computer locale o nell'archivio certificati personali), fare clic sulla scheda **Dettagli** , quindi nell'elenco dei **campi** fare clic su **identificazione personale**.

    > [!NOTE]
    > Quando si copia l'identificazione personale per l'utilizzo con lo strumento Rdpsign. exe, è necessario rimuovere tutti gli spazi.

-   È necessario specificare il file con estensione RDP (o i file) per la firma usando il nome file completo. I caratteri jolly non sono accettati.
-   I file di output firmati sovrascriveranno i file di input.
-   Se uno dei file con estensione RDP non può essere letto o scritto, lo strumento continuerà a eseguire il file successivo se vengono specificati più file.

## <a name="BKMK_examples"></a>Esempi
- Per firmare un file con estensione RDP denominato file1. RDP, passare alla cartella in cui è stato salvato il file con estensione RDP, quindi digitare quanto segue:
  ```
  rdpsign /sha1 hash file1.rdp
  ```
  > [!NOTE]
  > Il valore *hash* rappresenta l'identificazione personale del certificato SHA1 senza spazi.
- Per verificare se la firma digitale avrà esito positivo per un file con estensione RDP senza firmare effettivamente il file, digitare quanto segue:
  ```
  rdpsign /sha1 hash /l file1.rdp
  ```
- Per firmare più file con estensione RDP, separare i nomi dei file usando spazi. Ad esempio, per firmare più file con estensione RDP denominati file1. RDP, file2. RDP e file3. RDP, digitare quanto segue:
  ```
  rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
  ```
  ## <a name="see-also"></a>Vedi anche
  [Chiave di sintassi della riga di comando](command-line-syntax-key.md)
  [riferimento ai comandi di &#40;Servizi Desktop remoto Servizi terminal&#41; ](remote-desktop-services-terminal-services-command-reference.md)
