---
title: Impostazione del punteggio WinSAT sul server
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 20aafa957bd49f6522bfef98a2f8626dc5aab9df
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311502"
---
# <a name="set-the-winsat-score-on-the-server"></a>Impostazione del punteggio WinSAT sul server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per ottimizzare la risoluzione dei flussi video, è necessario impostare il Punteggio di CPU WinSAT per un server che esegue il sistema operativo Windows Server Essentials. A tale scopo, occorre creare e installare il file XML contenente le informazioni sul punteggio WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Come ottenere il punteggio WinSAT CPU  
 Con OPK, viene fornito all'utente un programma denominato WinServerSAT.exe in grado di rilevare il punteggio WinSAT CPU e di inserire tali informazioni nel file WinServerSAT.xml che il sistema operativo può quindi leggere.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Per ottenere il punteggio WinSAT CPU  
  
1. Copiare il Resources\WinServerSAT\\* nel supporto di ADK nel computer di riferimento.  
  
2. Sul computer di riferimento, aprire una finestra del prompt dei comandi con privilegi elevati.  
  
3. Se la cartella %ProgramFiles%\Windows Server\Bin\OEM non esiste, digitare il seguente comando e premere Invio.  
  
    **mkdir "%programmi%\Windows Server\Bin\OEM"**  
  
4. Digitare il seguente comando e premere Invio.  
  
    **WinServerSAT. exe "%programmi%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
   Nel seguente esempio viene mostrato il contenuto XML del file WinServerSAT.xml creato.  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 Dove *WinSAT_Score* viene sostituito con il valore individuato sul server.  
  
> [!IMPORTANT]
>  È necessario rimuovere i file WinServerSAT.exe, winsat.prx, winsat.wmv e WinSATEncode.wmv dal computer di riferimento prima di acquisire l'immagine.
