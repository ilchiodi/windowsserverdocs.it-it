---
title: 'Note sulla versione: problemi importanti in Windows Server 2019'
description: Riepiloga i problemi critici che richiedono soluzioni alternative per evitare gli arresti anomali, sporgenti, perdita di dati e di errore di installazione
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
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810735"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>Note sulla versione: problemi importanti in Windows Server 2019

>Si applica a: Windows Server 2019

Queste note sulla versione riepilogano i problemi più critici nel sistema operativo Windows Server 2019, incluse le soluzioni per evitare o risolvere i problemi, se noto. Per informazioni sulle modifiche da progettazione, le nuove funzionalità e le correzioni apportate in questa versione, vedere [What ' s New in Windows Server 2019](whats-new-19.md) e gli annunci dei team delle funzionalità specifiche. Se non diversamente specificato, ogni problema segnalato è applicabile a tutte le edizioni e opzioni di installazione di Windows Server 2019.  

Questo documento viene continuamente aggiornato. Non appena vengono rilevati problemi critici, le informazioni correlate vengono aggiunte immediatamente insieme alle soluzioni e alle correzioni disponibili.  

## <a name="release-notes"></a>Note sulla versione

I seguenti problemi noti sono presenti in Windows Server 2019.

| Titolo         | Descrizione                            |
| -----         | -----------                            |
| Menu di opzioni di installazione durante l'installazione di server è troncato testo in tedesco | Quando si esegue il programma di installazione dal supporto di server in lingua tedesca, nella finestra di selezione del sistema operativo intitolata: "Il sistema operativo da installare, selezionare" la descrizione per le opzioni di installazione di esperienza Desktop avrà mancanti e non corretti caratteri alla fine della frase. Ecco il testo in tedesco completo dovrebbe essere presente.<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>Ciò interessa solo il supporto in tedesco rilasciato al pubblico la disponibilità di Windows Server 2019, Windows Server, versione 1809 e Microsoft Hyper-V Server 2019.|
| Immagine della personalizzazione di Windows Server non corretto durante l'installazione di Windows Server, versione 1809 | Durante l'esperienza di installazione per Windows Server, versione 1809, l'immagine di sfondo su alcuni iniziale schermate illustra &quot;Windows Server 2019&quot;.  Come con Windows Server, versione 1709 e 1803, questo deve semplicemente indicare &quot;Windows Server&quot;.  Non vi sono altri problemi altrove nel prodotto e non ha alcun impatto sul prodotto Windows Server 2019.  Il problema è limitato a questa uno immagine durante l'installazione di Windows Server, versione 1809, disponibile solo per i clienti dei contratti multilicenza l'accesso a Volume License Service Center.<br/> |

### <a name="copyright"></a>Copyright

Questo documento viene fornito "così com'è". Le informazioni e le opinioni espresse nel presente documento, inclusi gli URL e altri riferimenti a siti Web Internet, sono soggette a modifiche senza preavviso.  

Il presente documento non implica la concessione di alcun diritto di proprietà intellettuale in relazione ai prodotti Microsoft. Sono consentiti la copia e l'uso del presente documento a fini di riferimento interno.

&copy; 2019 Microsoft Corporation. Tutti i diritti sono riservati.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server sono marchi o marchi registrati di Microsoft Corporation negli Stati Uniti e/o negli altri paesi.  

Questo prodotto contiene software per filtri grafici, parte del quale è basato sul lavoro dell'Independent JPEG Group.  


1.0  