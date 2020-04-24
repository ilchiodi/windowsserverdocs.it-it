---
title: Componenti tenant locali
description: Descrive i componenti locali della distribuzione RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: 849b0e3eb751c4e45a7c23da4230c7c4eb6bfcb1
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854704"
---
# <a name="tenant-on-premises-components"></a>Componenti tenant locali

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Le informazioni seguenti descrivono i componenti locali che costituiscono la distribuzione di hosting desktop.  
  
##  <a name="clients"></a>Client  
Per accedere a desktop e applicazioni ospitati, gli utenti devono usare client Desktop remoto che supportano Remote Desktop Protocol (RDP) 7.1 o versione successiva. In particolare, il client deve supportare Gateway Desktop remoto e Gestore connessione Desktop remoto. Per distribuire applicazioni sul desktop locale, il client deve anche supportare la funzionalità RemoteApp. Per ottenere una scalabilità gateway più elevata, il client deve supportare le connessioni di trasporto HTTP pure a Gateway Desktop remoto.  
  
Altre informazioni:  
[Novità di Gateway Desktop remoto in Windows Server 2012 R2](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Client Desktop remoto Microsoft](https://technet.microsoft.com/library/dn473009.aspx)  
[App Desktop remoto per Windows in Microsoft Store](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Desktop remoto Microsoft - App per Android in Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - Desktop remoto Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417?mt=12)  
[Desktop remoto Microsoft nell'App Store](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory  
Alcuni tenant più grandi e più sofisticati potrebbero scegliere di ospitare un server di Active Directory Domain Services (AD DS) in locale. In questo caso, il server di Active Directory Domain Services nell'ambiente del tenant in genere sarà una replica del server Active Directory Domain Services che si trova nella sede del tenant. Questa replica è supportata tramite la creazione di una rete virtuale nell'ambiente del tenant e l'uso della VPN di Azure per creare una connessione da sito a sito dalla rete locale del tenant alla rete virtuale del tenant nel data center di Azure.  
  
Altre informazioni:  
[Panoramica della rete virtuale di Microsoft Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[Creare una VNet di Gestione risorse con una connessione VPN da sito a sito usando il portale di Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  


