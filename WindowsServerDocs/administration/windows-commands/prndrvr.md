---
title: prndrvr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edf27852c13566ba8c7ca8c16d789d586e749160
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436216"
---
# <a name="prndrvr"></a>prndrvr

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usare la **prndrvr** comando per aggiungere, eliminare ed elencare i driver della stampante.

## <a name="syntax"></a>Sintassi
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|-a|Installa un driver.|
|-d|Consente di eliminare un driver.|
|-l|Elenca tutti i driver della stampante installati sul server specificato per il **-s** parametro. Se non si specifica un server, Windows sono elencati i driver della stampante installati sul computer locale.|
|-x|Elimina tutti i driver della stampante e ulteriori driver di stampante non è in uso da una stampante logica nel server specificato per il **-s** parametro. Se non si specifica un server da rimuovere dall'elenco, Windows elimina tutti i driver della stampante nel computer locale.|
|-m \<DrivermodelName\>|Specifica (nome) del driver da installare. I driver vengono spesso denominati per il modello di stampante che supportano. Vedere la documentazione della stampante per altre informazioni.|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|Specifica la versione del driver da installare. Vedere la descrizione della **-e**parametro per informazioni su cui sono disponibili versioni per l'ambiente. Se non si specifica una versione, la versione del driver appropriato per la versione di Windows in esecuzione nel computer in cui si installa il driver è installata.<br /><br />-versione **0** supporta Windows 95, Windows 98 e Windows Millennium edition.<br />-versione **1** supporta Windows NT 3.51.<br />-versione **2** supporta Windows NT 4.0.<br />-versione **3** supporta Windows Vista, Windows XP, Windows 2000 e i sistemi operativi Windows Server 2003. Si noti che questo è l'unica versione del driver della stampante che supporta Windows Vista.|
|-e \<ambiente >|Specifica l'ambiente per il driver da installare. Se non si specifica un ambiente, viene usato l'ambiente del computer in cui si installa il driver. I parametri di ambiente supportati sono:<br /><br />-    **"Windows NT x86"**<br />-    **"Windows x64"**<br />-    **"Windows IA64"**|
|-s \<nomeserver >|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene utilizzato il computer locale.|
|-u \<UserName > -w \<Password >|Specifica un account con autorizzazioni sufficienti per connettersi al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è anche possibile concedere le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario essere connessi con un account con queste autorizzazioni per il comando lavorare.|
|-h \<path>|Specifica il percorso del file di driver. Se non si specifica un percorso, viene utilizzato il percorso della posizione in cui è stato installato Windows.|
|-i \<Filename.inf>|Specifica il nome di file e percorso completo per il driver da installare. Se non si specifica un nome di file, lo script usa uno dei file della posta in arrivo della stampante. inf nella sottodirectory della directory Windows inf.<br /><br />Se il percorso del driver non è specificato, lo script cerca i file dei driver nel file CAB.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
- Il **prndrvr** comando è uno script Visual Basic che si trova nel %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Per usare questo comando, un prompt dei comandi, digitare **cscript** aggiungendo il percorso completo del file prndrvr, o cambiare le directory nella cartella appropriata.

  Ad esempio:
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- Se le informazioni fornite contengono spazi, utilizzare le virgolette intorno al testo (ad esempio, `"computer Name"`).
- X - opzione consente di eliminare tutti i driver della stampante aggiuntivi (driver installato per l'uso nei client alternativo in esecuzione versioni di Windows), anche se il driver primario è in uso. Se è installato il componente fax, questa opzione Elimina anche i driver fax. Il driver fax principale viene eliminato se non è in uso (ovvero, se è presente alcuna coda utilizzarlo). Se il driver fax principale viene eliminato, l'unico modo per abilitare nuovamente i fax è necessario reinstallare il componente fax.
- Se utilizzato senza parametri, **prndrvr** Visualizza la Guida della riga di comando per il **prndrvr** comando.

## <a name="BKMK_examples"></a>Esempi

Per elencare tutti i driver nel \\\printServer1 server, digitare:
```
cscript Prndrvr -l -s
```

Per aggiungere un driver della stampante Windows x64 versione 3 per il modello "modello di stampante Laser 1" della stampante con il file di informazioni C:\temp\Laserprinter1.inf driver per un driver archiviato nel tipo di cartella C:\temp:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

Per eliminare un driver della stampante Windows NT x86 versione 3 per "modello di stampante Laser 1", digitare:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[riferimenti ai comandi di stampa](print-command-reference.md)
