---
title: Autenticazione mediante chiave pubblica dispositivo aggiunto al dominio
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 8/18/2017
ms.openlocfilehash: 65f39f4900fcd99ef90b160d3db7099edb73bc1c
ms.sourcegitcommit: e94838df702ff0135ebe7c179b228aa67b772d35
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="domain-joined-device-public-key-authentication"></a>Autenticazione mediante chiave pubblica dispositivo aggiunto al dominio

>Si applica a: Windows Server 2016, Windows 10

Kerberos aggiunto il supporto per i dispositivi appartenenti a un dominio di accesso tramite inizio un certificato con Windows Server 2012 e Windows 8. Questa modifica consente ai fornitori di terze parti 3rd creare soluzioni per eseguire il provisioning e inizializzare i certificati per i dispositivi appartenenti a un dominio da utilizzare per l'autenticazione di dominio. 

## <a name="automatic-public-key-provisioning"></a>Pubblica chiave provisioning automatico

A partire da Windows 10 versione 1507 e Windows Server 2016, i dispositivi appartenenti a un dominio automaticamente il provisioning di una chiave pubblica associata a un controller di dominio (DC) di Windows Server 2016. Una volta che viene eseguito il provisioning di una chiave, Windows può utilizzare l'autenticazione mediante chiave pubblica al dominio.

### <a name="public-key-generation"></a>Generazione della chiave pubblica
Se il dispositivo esegue Credential Guard, una chiave pubblica viene creata protetta da Credential Guard. 

Se Credential Guard non è disponibile e un TPM, una chiave pubblica viene creata protetta dal TPM. 

Se non è disponibile, non viene generata una chiave e il dispositivo può solo eseguire l'autenticazione tramite password.

### <a name="provisioning-computer-account-public-key"></a>Provisioning di chiave pubblica di account computer
All'avvio di Windows, controlla se viene eseguito il provisioning di una chiave pubblica per l'account computer. In caso contrario, quindi genera una chiave pubblica associata e configurarla per l'account in Active Directory utilizzando un Windows Server 2016 o più controller di dominio. Se tutti i controller di dominio di livello inferiore, non viene eseguito il provisioning chiave.

### <a name="configuring-device-to-only-use-public-key"></a>Configurazione di dispositivo per utilizzare solo la chiave pubblica
Se l'impostazione di criteri di gruppo **il supporto per l'autenticazione del dispositivo utilizzando certificato** è impostato su **Force**, quindi il dispositivo deve per trovare un controller di dominio che esegue Windows Server 2016 o versione successiva per l'autenticazione. L'impostazione è in modelli amministrativi > sistema > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configurazione di dispositivo per utilizzare solo la password
Se l'impostazione di criteri di gruppo **il supporto per l'autenticazione del dispositivo utilizzando certificato** è disabilitato, la password viene sempre utilizzato. L'impostazione è in modelli amministrativi > sistema > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Autenticazione del dispositivo aggiunto al dominio utilizzando la chiave pubblica
Quando Windows dispone di un certificato per il dispositivo aggiunto al dominio, Kerberos prima l'autenticazione utilizzando il certificato e i tentativi di errore con password. In questo modo il dispositivo per l'autenticazione ai controller di dominio di livello inferiore.

Poiché le chiavi pubbliche provisioning automaticamente un certificato autofirmato, la convalida dei certificati ha esito negativo sui controller di dominio che non supportano il mapping account chiave attendibile. Per impostazione predefinita, Windows tenterà di autenticazione tramite password di dominio del dispositivo.


