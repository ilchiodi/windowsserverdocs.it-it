---
title: Aggiungere i tag di metadati richiesti per l'articolo correlati a Windows Server
description: Un elenco delle informazioni è necessario aggiungere come tag dei metadati nella parte superiore degli articoli correlati a Windows Server. I tag richiesti sono soggette a modifiche, in base ai requisiti di creazione di report sia team.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: f7c514def1353d44386b1bc53c8cabffe1e31fda
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461638"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>Aggiungere i tag di metadati richiesti per l'articolo correlati a Windows Server

Nella parte superiore di ogni articolo, ci sono metadati specifico che devono essere incluso per scopi SEO e rilevamento. I tag richiesti sono soggette a modifiche, basato su requisiti di report. Tuttavia, dovrebbe ricevere una notifica se è necessario aggiungere o rimuovere tutti i campi.

Dovrebbe essere simile al seguente, inclusi i tre segni meno (-) nella parte superiore e inferiore:

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they’re looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager’s Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>Esempio

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server-threshold
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```