---
title: Repository Software Linux per i prodotti Microsoft
description: Questo documento descrive come usare e installare i pacchetti software Linux per i prodotti Microsoft.
ms.custom: na
ms.prod: windows-server-threshold
ms.service: na
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: dbdbd0f436645f7e19c07e4f3278c5073636a547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831862"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Repository Software Linux per i prodotti Microsoft

## <a name="overview"></a>Panoramica
Microsoft si basa e supporta un'ampia gamma di prodotti software per i sistemi Linux e li rende disponibili tramite repository dei pacchetti APT e YUM standard. Questo documento descrive come configurare il repository nel proprio sistema Linux, in modo che si possa quindi installare/aggiornare il software Linux di Microsoft usando gli strumenti di gestione della distribuzione pacchetto standard.

Repository Software Linux di Microsoft è costituita da più repository secondario:

 - Prod-produzione il repository secondario designato per i pacchetti destinati all'uso nell'ambiente di produzione. Questi pacchetti in commercio sono supportati da Microsoft in base alle condizioni del contratto di supporto tecnico applicabili o programma che si dispone con Microsoft.

 - mssql-server - These repositories contain packages for Microsoft SQL Server on Linux - See also: [SQL Server in Linux](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux).

>[!Note]
I pacchetti nei repository software Linux sono soggetti a condizioni di licenza che si trova nei pacchetti. Leggere le condizioni di licenza prima di usare il pacchetto. L'installazione e l'uso del pacchetto costituisce l'accettazione di questi termini. Se non si accettano le condizioni di licenza, non usare il pacchetto.


## <a name="configuring-the-repositories"></a>Configurazione repository
I repository possono essere configurati automaticamente tramite l'installazione del pacchetto Linux che si applica alle distribuzioni di Linux e versione. I pacchetto installerà la configurazione del repository con la chiave pubblica GPG utilizzata dagli strumenti, ad esempio apt/zypper/yum per convalidare i pacchetti firmati e/o i metadati del repository.

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux (RHEL e varianti)

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14.04 (Trusty)

        wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update

 - Ubuntu 16.04 (Xenial)

        wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update

 - Ubuntu 16.10 (Yakkety)

        wget https://packages.microsoft.com/config/ubuntu/16.10/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update


### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>Configurazione manuale
Sono disponibili i file di configurazione del repository [packages.microsoft.com/config](https://packages.microsoft.com/config/). Il nome e il percorso di questi file possono essere individuati usando la convenzione di denominazione URI seguente:

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**Chiave del pacchetto e la firma del Repository**

 - Chiave pubblica GPG di Microsoft può essere scaricata da qui: [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - ID della chiave pubblica: Microsoft (firma della versione) <gpgsecurity@microsoft.com>
 - Impronta digitale della chiave pubblica: `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>Esempi:

 - RHEL/CentOS 7

        # Install repository configuration
        curl https://packages.microsoft.com/config/rhel/7/prod.repo > ./microsoft-prod.repo
        sudo cp ./microsoft-prod.repo /etc/yum.repos.d/

        # Install Microsoft's GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc > ./microsoft.asc
        sudo rpm --import ./microsoft.asc

 - Ubuntu 16.04

        # Install repository configuration
        curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
        sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

        # Install Microsoft GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
        sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/



