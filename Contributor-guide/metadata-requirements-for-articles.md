---
title: Aggiungere i tag di metadati necessari all'articolo relativo a Windows Server
description: Elenco delle informazioni che è necessario aggiungere come tag dei metadati all'inizio degli articoli correlati a Windows Server. I tag obbligatori sono soggetti a modifiche, in base ai requisiti del team e della creazione di report.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 9bedc7ec13aedab9cfaa655f537d5e431d20cccb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363965"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>Aggiungere i tag di metadati necessari all'articolo relativo a Windows Server

Nella parte superiore di ogni articolo sono disponibili metadati specifici che devono essere inclusi a scopo di rilevamento e SEO. I tag obbligatori sono soggetti a modifiche, in base ai requisiti per la creazione di report. Tuttavia, è necessario ricevere una notifica se è necessario aggiungere o rimuovere i campi.

Dovrebbe essere simile al seguente, inclusi i tre trattini (---) nella parte superiore e inferiore:

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they're looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager's Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>Esempio

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```