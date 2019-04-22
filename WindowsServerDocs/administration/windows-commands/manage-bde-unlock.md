---
title: gestire-bde sbloccare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a86267890449be2048221940e5955e49f30f99f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814602"
---
# <a name="manage-bde-unlock"></a>gestire-bde: sbloccare



Sblocca un'unità protetta da BitLocker utilizzando una password di ripristino o una chiave di ripristino. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

|Parametro|Value|Descrizione|
|---------|-----|-----------|
|-recoverypassword||Specifica una password di ripristino verrà utilizzata per sbloccare l'unità. Abbreviazione di: - rp|
||\<Password>|Rappresenta la password di ripristino che può essere utilizzata per sbloccare l'unità.|
|-recoverykey||Specifica un file di chiave esterna di ripristino verrà utilizzato per sbloccare l'unità. Abbreviazione: - rk|
||\<PathToExternalKeyFile>|Rappresenta il file di chiave esterna di ripristino che può essere utilizzato per sbloccare l'unità.|
||\<Drive>|Rappresenta una lettera di unità seguita da due punti.|
|-certificato||Il certificato utente locale di un certificato BitLocker unclock il volume si trova nell'archivio certificati utente locale. Abbreviazione di:-cert|
||<-cf PathToCertificateFile >|Percorso del file di certificato|
||<-ct CertificateThumbprint >|Identificazione personale del certificato che può includere facoltativamente il PIN (-pin).|
|-password||Visualizza un prompt dei comandi per la password per sbloccare il volume. Abbreviazione di: - pw|
|-computername||Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. Abbreviazione di: - cn|
||\<Nome >|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?||Consente di visualizzare breve guida al prompt dei comandi.|
|-help o -h||Visualizza la Guida completa al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **-sbloccare** comando per sbloccare unità E con un file di chiave di ripristino che è stato salvato in una cartella di backup in un'altra unità.
```
manage-bde –unlock E: -recoverykey "F:\Backupkeys\recoverykey.bek"
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Gestire-bde](manage-bde.md)