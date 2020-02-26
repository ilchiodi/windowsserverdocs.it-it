---
title: Autenticazione della chiave pubblica del dispositivo aggiunto al dominio
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: c9c4342281ee2036e152c8034fa72e421487a45b
ms.sourcegitcommit: 9bc7a0478d72944f714f8041fa4506e0d1ed0366
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/25/2020
ms.locfileid: "77607082"
---
# <a name="domain-joined-device-public-key-authentication"></a>Autenticazione della chiave pubblica del dispositivo aggiunto al dominio

>Si applica a: Windows Server 2016, Windows 10

Kerberos ha aggiunto il supporto per i dispositivi aggiunti a un dominio per l'accesso usando un certificato a partire da Windows Server 2012 e Windows 8. Questa modifica consente ai fornitori di terze parti di creare soluzioni per eseguire il provisioning e inizializzare certificati per i dispositivi aggiunti a un dominio da usare per l'autenticazione del dominio 

## <a name="automatic-public-key-provisioning"></a>Provisioning automatico della chiave pubblica

A partire da Windows 10 versione 1507 e Windows Server 2016, i dispositivi aggiunti a un dominio eseguono automaticamente il provisioning di una chiave pubblica associata a un controller di dominio Windows Server 2016 (DC). Una volta eseguito il provisioning di una chiave, Windows può usare l'autenticazione con chiave pubblica per il dominio.

### <a name="key-generation"></a>Generazione di chiavi
Se il dispositivo esegue Credential Guard, viene creata una coppia di chiavi pubblica/privata protetta da Credential Guard. 

Se Credential Guard non è disponibile e un TPM è, viene creata una coppia di chiavi pubblica/privata protetta dal TPM. 

Se nessuno dei due è disponibile, non viene generata una coppia di chiavi e il dispositivo può eseguire l'autenticazione solo usando la password.

### <a name="provisioning-computer-account-public-key"></a>Provisioning della chiave pubblica dell'account computer
All'avvio di Windows, verifica se viene effettuato il provisioning di una chiave pubblica per l'account computer. In caso contrario, viene generata una chiave pubblica associata e viene configurata per il relativo account in Active Directory utilizzando un controller di dominio Windows Server 2016 o versione successiva. Se tutti i controller di dominio sono di livello inferiore, non viene eseguito il provisioning di alcuna chiave.

### <a name="configuring-device-to-only-use-public-key"></a>Configurazione del dispositivo per usare solo la chiave pubblica
Se il Criteri di gruppo impostazione del **supporto per l'autenticazione del dispositivo con il certificato** è impostato su **forza**, il dispositivo deve trovare un controller di dominio che esegue Windows Server 2016 o versione successiva per l'autenticazione. L'impostazione è sotto Modelli amministrativi > sistema > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configurazione del dispositivo per usare solo la password
Se il Criteri di gruppo impostazione del **supporto per l'autenticazione del dispositivo mediante il certificato** è disabilitato, viene sempre utilizzata la password. L'impostazione è sotto Modelli amministrativi > sistema > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Autenticazione del dispositivo aggiunta a un dominio tramite chiave pubblica
Quando Windows dispone di un certificato per il dispositivo aggiunto a un dominio, Kerberos esegue prima l'autenticazione con il certificato e in caso di tentativi di errore con password. Ciò consente al dispositivo di eseguire l'autenticazione ai controller di dominio di livello inferiore.

Poiché le chiavi pubbliche con provisioning automatico hanno un certificato autofirmato, la convalida del certificato non riesce nei controller di dominio che non supportano il mapping dell'account attendibilità della chiave. Per impostazione predefinita, Windows ritenta l'autenticazione usando la password di dominio del dispositivo.


