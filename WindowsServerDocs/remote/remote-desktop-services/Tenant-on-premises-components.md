---
title: Componenti tenant locali
description: Descrive i componenti in locale nella distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: a01dbd12d76b1efa84e38f2ded38cfd613fb2ac4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857402"
---
# <a name="tenant-on-premises-components"></a>Componenti tenant locali

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Le informazioni seguenti descrivono i componenti in locale che costituiscono il distribuzione di hosting del desktop.  
  
##  <a name="clients"></a>Client  
Per accedere al desktop ospitati e le applicazioni, gli utenti devono usare i client Desktop remoto che supportano Remote Desktop Protocol (RDP) 7.1 o versione successiva. In particolare, il client deve supportare Gateway Desktop remoto e Gestore connessione Desktop remoto. Per distribuire le applicazioni sul desktop locale, il client deve anche supportare la funzionalità di RemoteApp. Per ottenere una scalabilità più elevata gateway, il client deve supportare le connessioni di trasporto HTTP pure per Gateway Desktop remoto.  
  
Altre informazioni:  
[Dispositivi abilitati per RemoteFX](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[Novità in Windows Server 2012 R2 Gateway Desktop remoto](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Client Desktop remoto Microsoft](https://technet.microsoft.com/library/dn473009.aspx)  
[App Desktop remoto per Windows in Microsoft Store](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Desktop remoto Microsoft - App per Android in Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - Desktop remoto Microsoft](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12)  
[Desktop remoto Microsoft in Store l'App](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory  
Alcuni tenant più grandi e più sofisticati potrebbero scegliere di ospitare un server di Active Directory Domain Services (AD DS) in locale. In questo caso, il server di Active Directory Domain Services nell'ambiente del tenant in genere sarà una replica di server di dominio Active Directory che si trova in sede del tenant. Questa è supportata la creazione di una rete virtuale nell'ambiente del tenant e usando la VPN di Azure per creare una connessione site-to-site nella rete locale del tenant alla rete virtuale del tenant in data center di Azure.  
  
Altre informazioni:  
[Panoramica di rete virtuale di Microsoft Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[Creare una risorsa di gestione della rete virtuale con una connessione Site-to-Site VPN usando il portale di Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  


