---
title: prndrvr
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 05e03a4b0b5d686c8fbd1646a775c7f0e5c95706
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372124"
---
# <a name="prndrvr"></a>prndrvr

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usare il comando **prndrvr** per aggiungere, eliminare ed elencare i driver della stampante.

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
|-d|Elimina un driver.|
|-l|Elenca tutti i driver della stampante installati nel server specificato dal parametro **-s** . Se non si specifica un server, Windows elenca i driver della stampante installati nel computer locale.|
|-x|Elimina tutti i driver della stampante e i driver della stampante aggiuntivi non utilizzati da una stampante logica sul server specificato dal parametro **-s** . Se non si specifica un server da rimuovere dall'elenco, Windows eliminerà tutti i driver della stampante non utilizzati nel computer locale.|
|-m \<DrivermodelName\>|Specifica (per nome) il driver che si desidera installare. I driver sono spesso denominati per il modello di stampante supportato. Per ulteriori informazioni, vedere la documentazione della stampante.|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|Specifica la versione del driver che si desidera installare. Per informazioni sulle versioni disponibili per l'ambiente, vedere la descrizione del parametro **-e**. Se non si specifica una versione, la versione del driver appropriata per la versione di Windows in esecuzione nel computer in cui si sta installando il driver viene installata.<br /><br />-la versione **0** supporta Windows 95, Windows 98 e Windows Millennium Edition.<br />-la versione **1** supporta Windows NT 3,51.<br />-la versione **2** supporta Windows NT 4,0.<br />-la versione **3** supporta i sistemi operativi Windows Vista, Windows XP, Windows 2000 e windows Server 2003. Si noti che questa è l'unica versione del driver della stampante supportata da Windows Vista.|
|-e \<ambiente >|Specifica l'ambiente per il driver che si desidera installare. Se non si specifica un ambiente, viene utilizzato l'ambiente del computer in cui viene installato il driver. I parametri di ambiente supportati sono:<br /><br />-    **"Windows NT x86"**<br />-    **"Windows x64"**<br />-    **"Windows IA64"**|
|-s \<nomeserver >|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene usato il computer locale.|
|-u \<nome utente >-w \<password >|Specifica un account con le autorizzazioni per la connessione al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è possibile concedere anche le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario effettuare l'accesso con un account con le autorizzazioni necessarie per il funzionamento del comando.|
|-h \<percorso >|Specifica il percorso del file del driver. Se non si specifica un percorso, viene utilizzato il percorso della posizione in cui è stato installato Windows.|
|-i \<Filename.inf>|Specifica il percorso completo e il nome file per il driver che si desidera installare. Se non si specifica un nome di file, lo script usa uno dei file di Printer. inf della posta in arrivo nella sottodirectory INF della directory di Windows.<br /><br />Se il percorso del driver non è specificato, lo script cerca i file del driver nel file CAB del driver.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
- Il comando **prndrvr** è uno script Visual Basic che si trova nella directory%windir%\system32\. printing_Admin_Scripts\\<language>. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file prndrvr o passare alla cartella appropriata.

  Ad esempio:
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio `"computer Name"`.
- L'opzione-x Elimina tutti i driver della stampante aggiuntivi, ovvero i driver installati per l'uso nei client che eseguono versioni alternative di Windows, anche se il driver primario è in uso. Se il componente fax è installato, questa opzione Elimina anche i driver fax. Il driver fax primario viene eliminato se non è in uso, ovvero se non è presente alcuna coda. Se il driver fax primario viene eliminato, l'unico modo per riattivare il fax è reinstallare il componente fax.
- Usato senza parametri, **prndrvr** Visualizza la guida della riga di comando per il comando **prndrvr** .

## <a name="BKMK_examples"></a>Esempi

Per elencare tutti i driver nel server \\\printServer1, digitare:
```
cscript Prndrvr -l -s
```

Per aggiungere un driver della stampante Windows x64 versione 3 per il modello di stampante "Laser Printer Model 1" utilizzando il file di informazioni del driver C:\temp\Laserprinter1.inf per un driver archiviato nel tipo di cartella C:\Temp:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

Per eliminare un driver della stampante Windows NT x86 versione 3 per "Laser Printer Model 1", digitare:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[riferimento al comando stampa](print-command-reference.md)
