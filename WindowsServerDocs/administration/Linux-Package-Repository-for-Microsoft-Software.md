---
title: Repository software Linux per prodotti Microsoft
description: Questo documento descrive come usare e installare i pacchetti software Linux per i prodotti Microsoft.
ms.custom: na
ms.prod: windows-server
ms.service: na
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: e32c11dac1d887ba0ae0192bb658f71ece77a42c
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947241"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Repository software Linux per prodotti Microsoft

## <a name="overview"></a>Panoramica
Microsoft compila e supporta un'ampia gamma di prodotti software per sistemi Linux e li rende disponibili tramite repository di pacchetti APT e YUM standard. Questo documento descrive come configurare il repository nel sistema Linux, in modo da poter installare o aggiornare il software Linux Microsoft usando gli strumenti di gestione dei pacchetti standard della distribuzione.

Il repository software Linux Microsoft è costituito da più repository secondari:

 - prod: il repository secondario di produzione è designato per i pacchetti destinati all'uso nell'ambiente di produzione. Questi pacchetti sono supportati commercialmente da Microsoft in base ai termini del contratto o del programma di supporto applicabile con Microsoft.

 - MSSQL-Server: questi repository contengono pacchetti per Microsoft SQL Server in Linux. vedere anche: [SQL Server in Linux](https://www.microsoft.com/sql-server/sql-server-vnext-including-Linux).

> [!Note]
> I pacchetti nei repository software Linux sono soggetti alle condizioni di licenza presenti nei pacchetti. Prima di usare il pacchetto, leggere le condizioni di licenza. L'installazione e l'uso del pacchetto costituiscono accettazione di tali condizioni. Se non si accettano le condizioni di licenza, non usare il pacchetto.


## <a name="configuring-the-repositories"></a>Configurazione dei repository
I repository possono essere configurati automaticamente installando il pacchetto Linux applicabile alla distribuzione e alla versione di Linux. Il pacchetto installerà la configurazione del repository insieme alla chiave pubblica GPG usata da strumenti come apt/yum/zypper per convalidare i pacchetti firmati e/o i metadati del repository.

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux (RHEL e varianti)

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14,04 (Trusty)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/14.04/prod
        sudo apt-get update

 - Ubuntu 16,04 (Xenial)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod
        sudo apt-get update

 - Ubuntu 18,04 (Bionic)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
        sudo apt-get update

 - Ubuntu 18,10 (cosmico)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.10/prod
        sudo apt-get update

 - Ubuntu 19,04 (disco)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/19.04/prod
        sudo apt-get update

### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>Configurazione manuale
I file di configurazione del repository sono disponibili da [packages.Microsoft.com/config](https://packages.microsoft.com/config/). Il nome e il percorso di questi file possono essere individuati usando la convenzione di denominazione URI seguente:

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**Chiave di firma del pacchetto e del repository**

 - La chiave pubblica GPG di Microsoft può essere scaricata qui: [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - ID chiave pubblica: Microsoft (firma di rilascio) <gpgsecurity@microsoft.com>
 - Impronta digitale chiave pubblica: `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>Di seguito sono riportati alcuni esempi.

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



