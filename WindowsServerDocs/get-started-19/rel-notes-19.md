---
title: 'Note sulla versione: problemi importanti di Windows Server 2019'
description: Questo articolo presenta una sintesi dei problemi critici che richiedono soluzioni alternative per evitare arresti anomali, blocchi, errori di installazione o perdita di dati
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 515255c301d343aa1b83bcfb506f2e3baa6ca969
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810735"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>Note sulla versione: problemi importanti di Windows Server 2019

>Si applica a: Windows Server 2019

Queste note sulla versione presentano una sintesi dei problemi più critici del sistema operativo Windows Server 2019, incluse le soluzioni per evitare o risolvere i problemi, se noti. Per informazioni sulle modifiche da progettazione, le nuove funzionalità e le correzioni apportate in questa versione, vedi [Novità di Windows Server 2019](whats-new-19.md) e gli annunci dei team addetti alle funzionalità specifiche. Se non diversamente specificato, ogni problema segnalato è applicabile a tutte le edizioni e opzioni di installazione di Windows Server 2019.  

Questo documento viene continuamente aggiornato. Non appena vengono rilevati problemi critici, le informazioni correlate vengono aggiunte immediatamente insieme alle soluzioni e alle correzioni disponibili.  

## <a name="release-notes"></a>Note sulla versione

I problemi noti seguenti sono presenti in Windows Server 2019.

| Title         | Descrizione                            |
| -----         | -----------                            |
| Il menu dell'opzione di installazione durante l'installazione del server ha il testo in lingua tedesca troncato | Durante l'installazione dal supporto per il server in lingua tedesca, nella finestra per la selezione del sistema operativo la descrizione delle opzioni di installazione di Esperienza desktop presenterà caratteri errati e mancanti alla fine della frase. Ecco come dovrebbe essere visualizzato il testo completo in lingua tedesca.<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>Questo problema influisce solo sul supporto in lingua tedesca rilasciato in disponibilità pubblica con Windows Server 2019, Windows Server versione 1809 e Microsoft Hyper-V Server 2019.|
| L'immagine di personalizzazione di Windows Server durante l'installazione di Windows Server 1809 non è corretta | Durante l'esperienza di installazione di Windows Server versione 1809, nell'immagine di sfondo di alcune schermate iniziali viene visualizzato &quot;Windows Server 2019&quot;.  Come per Windows Server versioni 1709 e 1803, l'immagine dovrebbe semplicemente indicare &quot;Windows Server&quot;.  Questo problema non ha alcun impatto su altri aspetti del prodotto e su Windows Server 2019.  Il problema è limitato a questa immagine durante l'installazione di Windows Server versione 1809, disponibile solo per i clienti dei contratti multilicenza che accedono a Volume License Service Center.<br/> |

### <a name="copyright"></a>Copyright

Questo documento viene fornito "così com'è". Le informazioni e le opinioni espresse nel presente documento, inclusi gli URL e altri riferimenti a siti Web Internet, sono soggette a modifiche senza preavviso.  

Il presente documento non implica la concessione di alcun diritto di proprietà intellettuale in relazione ai prodotti Microsoft. Sono consentiti la copia e l'uso del presente documento a fini di riferimento interno.

&copy; 2019 Microsoft Corporation. Tutti i diritti sono riservati.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server sono marchi o marchi registrati di Microsoft Corporation negli Stati Uniti e/o negli altri paesi.  

Questo prodotto contiene software per filtri grafici, parte del quale è basato sul lavoro dell'Independent JPEG Group.  


1.0  
