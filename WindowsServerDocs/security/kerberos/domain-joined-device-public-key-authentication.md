---
title: Autenticazione della chiave pubblica del dispositivo aggiunto al dominio
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: 80906e7cfe3740200938704a4b4eaf0759af303a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885032"
---
# <a name="domain-joined-device-public-key-authentication"></a>Autenticazione della chiave pubblica del dispositivo aggiunto al dominio

>Si applica a: Windows Server 2016, Windows 10

Kerberos aggiunto il supporto per i dispositivi aggiunti a un dominio per l'accesso usando un certificato che inizia con Windows Server 2012 e Windows 8. Questa modifica consente a fornitori di terze parti 3rd creare soluzioni per eseguire il provisioning e l'inizializzazione dei certificati per i dispositivi aggiunti a un dominio da usare per l'autenticazione del dominio. 

## <a name="automatic-public-key-provisioning"></a>Pubblica chiavi il provisioning automatico

A partire da Windows 10 versione 1507 e Windows Server 2016, i dispositivi aggiunti a un dominio automaticamente il provisioning di una chiave pubblica associata a un controller di dominio (DC) di Windows Server 2016. Una volta che viene eseguito il provisioning di una chiave, Windows può usare l'autenticazione con chiave pubblica al dominio.

### <a name="public-key-generation"></a>Generazione della chiave pubblica
Se il dispositivo è in esecuzione di Credential Guard, una chiave pubblica viene creata sono protetta dai Credential Guard. 

Se di Credential Guard non è disponibile ed è un modulo TPM, una chiave pubblica viene creata protetta dal TPM. 

Se non è disponibile, non viene generata una chiave e il dispositivo può solo eseguire l'autenticazione tramite password.

### <a name="provisioning-computer-account-public-key"></a>Provisioning chiave pubblica dell'account computer
Quando Windows viene avviato, controlla se viene eseguito il provisioning di una chiave pubblica per l'account del computer. In caso contrario, quindi genera una chiave pubblica associata e lo configura per il relativo account in Active Directory utilizzando un Windows Server 2016 o versione successiva controller di dominio. Se tutti i controller di dominio sono di livello inferiore, non è presente alcuna chiave.

### <a name="configuring-device-to-only-use-public-key"></a>Configurazione di dispositivo di usare solo la chiave pubblica
Se l'impostazione di criteri di gruppo **supporto per l'autenticazione del dispositivo con certificato** è impostata su **Force**, quindi il dispositivo deve trovare un controller di dominio che esegue Windows Server 2016 o versioni successive per eseguire l'autenticazione. L'impostazione si trova in modelli amministrativi > sistema > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configurazione di dispositivo di usare solo la password
Se l'impostazione di criteri di gruppo **supporto per l'autenticazione del dispositivo con certificato** è disabilitata, la password viene sempre usata. L'impostazione si trova in modelli amministrativi > sistema > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Autenticazione dei dispositivi aggiunti a un dominio con la chiave pubblica
Quando Windows ha un certificato per il dispositivo aggiunto al dominio, Kerberos prima l'autenticazione usando il certificato e tentativi di errore con la password. Ciò consente al dispositivo di eseguire l'autenticazione ai controller di dominio di livello inferiore.

Poiché le chiavi pubbliche automaticamente sottoposte a provisioning dispone di un certificato autofirmato, la convalida dei certificati ha esito negativo nei controller di dominio che non supportano il mapping di account di attendibilità chiavi. Per impostazione predefinita, Windows esegue l'autenticazione tramite password di dominio del dispositivo.


