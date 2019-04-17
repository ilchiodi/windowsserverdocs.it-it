---
title: 'Note sulla versione: problemi importanti di Windows Server 2019'
description: Riepilogano i problemi critici che richiedono soluzioni alternative per evitare gli arresti anomali, rispetto ad altri elementi, perdita di dati e di errore di installazione
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: d5d84bb1cd204a5419271cc3668343f8ac9c9a54
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149891"
---
# Note sulla versione: problemi importanti di Windows Server 2019

>Si applica a: Windows Server 2019

Queste note sulla versione presentano una sintesi dei problemi più critici in Windows Server&reg; sistema operativo 2019, incluse le soluzioni per evitare o risolvere i problemi, se noti. Per informazioni sulle modifiche da progettazione, le nuove funzionalità e le correzioni apportate in questa versione, vedi [Novità in Windows Server 2019](whats-new-19.md) e gli annunci dei team addetti alle funzionalità specifiche. Se non diversamente specificato, ogni problema segnalato è applicabile a tutte le edizioni e opzioni di installazione di Windows Server 2019.  

Questo documento viene continuamente aggiornato. Non appena vengono rilevati problemi critici, le informazioni correlate vengono aggiunte immediatamente insieme alle soluzioni e alle correzioni disponibili.  
  
## Note sulla versione
I seguenti problemi noti sono presenti in Windows Server 2019. 
<table border="1" rules="rows">
  <thead align="left" valign="middle">
    <tr>
      <th>Titolo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody align="left" valign="middle">
    <tr>
      <td>Menu di opzioni di installazione durante l'installazione di server è troncata testo tedesco</td>
      <td>Quando si esegue il programma di installazione dal supporto di server tedeschi, nella finestra di selezione del sistema operativo intitolata: "Il sistema operativo che si desidera installare, seleziona" la descrizione per le opzioni di installazione di esperienza Desktop avrà caratteri mancanti e non corretti alla fine della frase. Ecco il testo completo tedesco come dovrebbe apparire.  
      <br/>
      <p><i>Durch diese opzione wird die vollständige grafische installiert Umgebung von Windows, wodurch zusätzlicher Speicherplatz verbraucht wird. SIE kann hilfreich sein, wenn Sie preparare Windows Desktop verwenden möchten o über eine App verfügen, die die grafische Umgebung benötigt.</i> </p>
      <p>Questo incide solo il supporto tedesco rilasciato in pubblica la disponibilità di Windows Server 2019, Windows Server, versione 1809 e Microsoft Hyper-V Server 2019.</p></td>
    </tr>
    <tr>
      <td>Immagine di Windows Server personalizzazione non corretto durante l'installazione di Windows Server, versione 1809  </td>
      <td>Durante l'esperienza di installazione per Windows Server, versione 1809, l'immagine di sfondo in alcune schermate iniziale Mostra "Windows Server 2019".  Come con Windows Server, versione 1709 e 1803, questo semplicemente dovrebbe essere visualizzato "Server di Windows".  Non sono altri impatti altri del prodotto e non ha alcun impatto per il prodotto Windows Server 2019.  Il problema è limitato a questo un'immagine durante l'installazione di Windows Server, versione 1809, disponibile solo per accedere al centro servizi per contratti multilicenza licenza clienti per contratti multilicenza.  
      </td>
    </tr>
  </tbody>
</table>


### Copyright  
Questo documento viene fornito "così com'è". Le informazioni e le opinioni espresse nel presente documento, inclusi gli URL e altri riferimenti a siti Web Internet, sono soggette a modifiche senza preavviso.  

Il presente documento non implica la concessione di alcun diritto di proprietà intellettuale in relazione ai prodotti Microsoft. È possibile copiare e usare questo documento per fini di riferimento interno.  

&copy;2019 Microsoft Corporation. Tutti i diritti sono riservati.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server sono marchi o marchi registrati di Microsoft Corporation negli Stati Uniti e/o negli altri paesi.  

Questo prodotto contiene software per filtri grafici, parte del quale è basato sul lavoro dell'Independent JPEG Group.  


1.0  
