---
title: Importare i pacchetti di dati nel server della cache ospitata (facoltativo)
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 440ef1e04143cba09213ffea634aa9d4fea51dab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888002"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importare i pacchetti di dati nel Server Cache ospitata \(facoltativo\)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa procedura per importare i pacchetti di dati e a precaricare contenuto nei server cache ospitata.

Questa procedura è facoltativa perché non si deve prehash e precaricamento contenuto nei server cache ospitata.

In caso non pre\-carico contenuto, i dati vengono aggiunti alla cache ospitata automaticamente come client scaricano tramite una connessione WAN.

Per eseguire questa procedura, è necessario essere membri del gruppo Administrators.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Per importare i pacchetti di dati nel server cache ospitata  

1. Nel computer del server, aprire Windows PowerShell con privilegi di amministratore.

2. Digitare il comando seguente, sostituendo il valore per il parametro-Path con il percorso della cartella in cui sono archiviati i pacchetti di dati e quindi premere INVIO.

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. Se si dispone di più di un server cache ospitata in cui si desidera precaricare contenuto, è possibile eseguire questa procedura su ogni server cache ospitata.

Per continuare con questa Guida, vedere [configurare Client ospitato Cache rilevamento automatico dal punto di connessione servizio](10-Bc-Client-By-Scp.md).
