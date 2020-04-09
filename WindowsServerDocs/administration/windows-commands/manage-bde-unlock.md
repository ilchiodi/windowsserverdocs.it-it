---
title: 'gestione: sblocco BDE'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e26952ca8c2b20cb0cb8efa167fca81e27b692a0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839738"
---
# <a name="manage-bde-unlock"></a>Manage-bde: sblocco



Sblocca un'unità protetta da BitLocker utilizzando una password di ripristino o una chiave di ripristino. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Valore|Descrizione|
|---------|-----|-----------|
|-recoverypassword||Specifica una password di ripristino verrà utilizzata per sbloccare l'unità. Abbreviazione di: - rp|
||> password \<|Rappresenta la password di ripristino che può essere utilizzata per sbloccare l'unità.|
|-recoverykey||Specifica un file di chiave esterna di ripristino verrà utilizzato per sbloccare l'unità. Abbreviazione: - rk|
||\<PathToExternalKeyFile >|Rappresenta il file di chiave esterna di ripristino che può essere utilizzato per sbloccare l'unità.|
||Unità \<>|Rappresenta una lettera di unità seguita da due punti.|
|-certificato||Il certificato utente locale di un certificato BitLocker unclock il volume si trova nell'archivio certificati utente locale. Abbreviazione di:-cert|
||<-cf PathToCertificateFile >|Percorso del file di certificato|
||<-ct CertificateThumbprint >|Identificazione personale del certificato che può includere facoltativamente il PIN (-pin).|
|-password||Visualizza un prompt dei comandi per la password per sbloccare il volume. Abbreviazione di: - pw|
|-computername||Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. Abbreviazione di: - cn|
||Nome \<>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?||Visualizza una breve guida al prompt dei comandi.|
|-Help o-h||Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **-sbloccare** comando per sbloccare unità E con un file di chiave di ripristino che è stato salvato in una cartella di backup in un'altra unità.
```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)