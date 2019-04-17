---
title: Impostazione del punteggio WinSAT sul Server
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 77866acccac13ac48da8779700c8654f2c7f3277
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="set-the-winsat-score-on-the-server"></a>Impostazione del punteggio WinSAT sul Server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È consigliabile impostare il punteggio WinSAT CPU per un server che esegue il sistema operativo Windows Server Essentials per ottimizzare la risoluzione di flusso video. Farlo creando e installando il file XML che contiene le informazioni sul punteggio WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Ottenere il punteggio WinSAT CPU  
 Vengono fornite un programma con OPK denominato WinServerSAT.exe che consente di individuare il punteggio WinSAT CPU e inserire tali informazioni nel file WinServerSAT.xml che legge il sistema operativo.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Per ottenere il punteggio WinSAT CPU  
  
1.  Copiare Resources\WinServerSAT\\ * nel supporto ADK nel computer di riferimento.  
  
2.  Nel computer di riferimento, aprire una finestra del prompt dei comandi con privilegi elevata.  
  
3.  Se la cartella %ProgramFiles%\Windows Server\Bin\OEM non esiste, digitare il comando seguente e quindi premere INVIO.  
  
     **mkdir "%ProgramFiles%\Windows server\bin\oem."**  
  
4.  Digitare il comando seguente e quindi premere INVIO.  
  
     **"%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml" WinServerSAT.exe**  
  
 L'esempio seguente mostra il contenuto XML del file WinServerSAT.xml creato.  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 Dove *WinSAT_Score* viene sostituito con il valore che viene individuato sul server.  
  
> [!IMPORTANT]
>  È necessario rimuovere WinServerSAT.exe, winsat.prx, winsat.wmv e WinSATEncode.wmv file dal computer di riferimento prima di acquisire l'immagine.
