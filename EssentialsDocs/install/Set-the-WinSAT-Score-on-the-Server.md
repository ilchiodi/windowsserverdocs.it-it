---
title: Impostazione del punteggio WinSAT sul server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819952"
---
# <a name="set-the-winsat-score-on-the-server"></a>Impostazione del punteggio WinSAT sul server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È consigliabile impostare il punteggio WinSAT CPU per un server che esegue il sistema operativo Windows Server Essentials per ottimizzare la risoluzione di streaming video. A tale scopo, occorre creare e installare il file XML contenente le informazioni sul punteggio WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Come ottenere il punteggio WinSAT CPU  
 Con OPK, viene fornito all'utente un programma denominato WinServerSAT.exe in grado di rilevare il punteggio WinSAT CPU e di inserire tali informazioni nel file WinServerSAT.xml che il sistema operativo può quindi leggere.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Per ottenere il punteggio WinSAT CPU  
  
1.  Copiare il Resources\WinServerSAT\\* nel supporto ADK nel computer di riferimento.  
  
2.  Sul computer di riferimento, aprire una finestra del prompt dei comandi con privilegi elevati.  
  
3.  Se la cartella %ProgramFiles%\Windows Server\Bin\OEM non esiste, digitare il seguente comando e premere Invio.  
  
     **mkdir "%ProgramFiles%\Windows Server\Bin\OEM"**  
  
4.  Digitare il seguente comando e premere Invio.  
  
     **WinServerSAT.exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
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
