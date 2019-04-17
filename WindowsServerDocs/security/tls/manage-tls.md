---
title: Gestire Transport Layer Security (TLS)
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 06/05/2017
ms.openlocfilehash: e26d1f833dbb7219251947f1cb3cb09def958aea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-transport-layer-security-tls"></a>Gestire Transport Layer Security (TLS)

## <a name="configuring-tls-cipher-suite-order"></a>Configurazione ordine dei pacchetti di crittografia TLS

Diverse versioni di Windows supportano diversi pacchetti di crittografia TLS e ordine di priorità. Vedere [pacchetti di crittografia di TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) per l'ordine predefinito è supportata dal Provider Schannel Microsoft nelle diverse versioni di Windows.

> [!NOTE] 
> È inoltre possibile modificare l'elenco dei pacchetti di crittografia tramite le funzioni CNG, vedere [assegnare la priorità ai pacchetti di crittografia Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) per informazioni dettagliate.

Modifiche per l'ordine dei pacchetti di crittografia TLS verranno applicate al successivo avvio. Fino al riavvio o arresto, l'ordine esistente verranno applicati.

> [!WARNING] 
> Aggiornare le impostazioni del Registro di sistema per l'ordine di priorità predefinito non è supportata e potrebbe essere necessario reimpostare con gli aggiornamenti di manutenzione. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>La configurazione di ordine dei pacchetti di crittografia TLS mediante criteri di gruppo

È possibile utilizzare le impostazioni di criteri di gruppo ordine Suite crittografia SSL per configurare l'ordine di suite crittografia TLS predefinito.

1.  Dalla Console Gestione criteri di gruppo, Vai a **configurazione Computer** > **modelli amministrativi** > **reti** > **le impostazioni di configurazione SSL**.
2.  Fare doppio clic su **ordine dei pacchetti di crittografia SSL**, quindi fare clic su di **abilitato** opzione.
3.  Fare doppio clic su **SSL Cipher Suites** e selezionare **Seleziona tutto** dal menu a comparsa.

    ![Impostazione di criteri di gruppo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4.  Il testo selezionato e scegliere **copia** dal menu a comparsa.
5.  Incollare il testo in un editor di testo, ad esempio notepad.exe e aggiornamento con il nuovo elenco ordine di crittografia suite.

    > [!NOTE]
    > L'elenco di ordine suite di crittografia TLS deve essere in formato delimitato strict. Ogni stringa di crittografia suite termina con una virgola (,) sul lato destro di esso. 

    > Inoltre, l'elenco dei pacchetti di crittografia è limitato a 1.023 caratteri.

6.  Sostituire l'elenco nel **SSL Cipher Suites** con l'elenco ordinato aggiornato.
7.  Fare clic su **OK** o **applicare**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configurare l'ordine dei pacchetti di crittografia TLS mediante MDM

Il provider CSP criteri di Windows 10 supporta la configurazione dei pacchetti di crittografia TLS. Vedere [crittografia/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) per ulteriori informazioni.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurare l'ordine dei pacchetti di crittografia TLS utilizzando i cmdlet PowerShell di TLS

Il modulo PowerShell di TLS supporta il recupero dell'elenco ordinato dei pacchetti di crittografia TLS, la disattivazione di un pacchetto di crittografia e l'abilitazione di un pacchetto di crittografia. Vedere [modulo TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) per ulteriori informazioni.

## <a name="configuring-tls-ecc-curve-order"></a>Configurazione dell'ordine di curva ECC TLS 

A partire da Windows 10 e Windows Server 2016, ECC curva è possibile configurare indipendente dall'ordine crittografia suite. Se l'elenco contiene suffissi a curva ellittica ordine dei pacchetti di crittografia TLS, verrà sostituiti dal nuovo ordine di priorità a curva ellittica, quando abilitato. Ciò consente alle organizzazioni di usare un oggetto Criteri di gruppo per configurare diverse versioni di Windows con lo stesso ordine di pacchetti di crittografia.

> [!NOTE]
> Prima di Windows 10, le stringhe di crittografia suite sono stati aggiunti con la curva ellittica per determinare la priorità di curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Gestione curve ECC Windows utilizzando CertUtil

A partire da Windows 10 e Windows Server 2016, Windows offre gestione parametro a curva ellittica però il certuil.exe utilità della riga di comando. Parametri di curva ellittica vengono archiviati nel bcryptprimitives.dll. Utilizzando certutil.exe, gli amministratori possono aggiungere e rimuovere i parametri di curva in e da Windows, rispettivamente. Certutil.exe archivia i parametri di curva in modo sicuro nel Registro di sistema. Windows può iniziare a usare i parametri di curva per il nome associato la curva.    

#### <a name="displaying-registered-curves"></a>Visualizzare le curve registrate

Utilizzare il seguente comando certutil.exe per visualizzare un elenco delle curve registrato per il computer corrente.

```powershell
certutil.exe –displayEccCurve
```

![Certutil visualizzazione curve](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figura 1 Certutil.exe output per visualizzare l'elenco delle curve registrate.*

#### <a name="adding-a-new-curve"></a>Aggiunta di una curva di nuovo

Le organizzazioni possono creare e usare parametri di curva cercarli da altre entità attendibili.  
Gli amministratori che desiderano usare queste nuove curve in Windows è necessario aggiungere la curva.  
Utilizzare il comando certutil.exe seguente per aggiungere una curva al computer corrente:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- Il **curveName** argomento rappresenta il nome della curva in cui sono stati aggiunti i parametri di curva.
- Il **curveParameters** argomento rappresenta il nome del file di un certificato che contiene i parametri delle curve di cui si desidera aggiungere.
- Il **curveOid** argomento rappresenta il nome di un certificato che contiene l'OID dei parametri di curva si desidera aggiungere (facoltativo).
- Il **curveType** argomento rappresenta il valore decimale della curva denominato dal [CE denominato al Registro di sistema di curva](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (facoltativo).

![Certutil aggiungere curve](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 aggiungendo una curva utilizzando certutil.exe.*

#### <a name="removing-a-previously-added-curve"></a>Rimozione di una curva aggiunta in precedenza

Gli amministratori possono rimuovere una curva aggiunta in precedenza utilizzando il comando certutil.exe seguenti:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Impossibile utilizzare una curva denominata dopo che un amministratore rimuove la curva dal computer.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Gestione curve ECC Windows utilizzando criteri di gruppo

Le organizzazioni possono distribuire ai parametri di curva a enterprise, dominio, computer utilizzando criteri di gruppo e l'estensione del Registro di preferenze criteri di gruppo.  
Il processo per la distribuzione di una curva è:

1.  In Windows 10 e Windows Server 2016, usare **certutil.exe** per aggiungere una nuova curva denominata registrata a Windows.
2.  Da tale computer stesso, aprire la Console Gestione criteri di gruppo (GPMC), creare un nuovo oggetto Criteri di gruppo e modificarlo.
3.  Passare a **configurazione Computer | Preferenze | Impostazioni di Windows | Registro di sistema**.  Fare doppio clic su **Registro di sistema**. Passa il puntatore su **New** e seleziona **elemento della raccolta**. Rinominare l'elemento della raccolta in base al nome della curva stessa. È necessario creare un elemento di raccolta del Registro di sistema per ogni chiave del Registro di sistema in *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurare la raccolta del Registro di sistema delle preferenze di criteri di gruppo appena creato mediante l'aggiunta di un nuovo **elemento Registro di sistema** per ogni valore del Registro di sistema è elencato in *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\ [curveName]*.
5.  Distribuire l'oggetto Criteri di gruppo che contiene i computer Windows 10 e Windows Server 2016 che deve ricevere le curve denominate nuovo elemento raccolta del Registro di sistema dei criteri di gruppo.

    ![GPP distribuire curve](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 utilizzando criteri di gruppo per distribuire le curve*

## <a name="managing-tls-ecc-order"></a>Gestione ordine ECC TLS

A partire da Windows 10 e Windows Server 2016, è possibile utilizzare impostazioni criteri di gruppo di ordine curva ECC configurare il valore predefinito dell'ordine di curva di TLS ECC. Utilizzando ECC generico e questa impostazione, le organizzazioni possa aggiungere i propri denominato curve (che vengono approvate per l'utilizzo con TLS) per il sistema operativo attendibile, quindi quelli denominato curve per l'impostazione di criteri di gruppo priorità curva per garantire che vengono utilizzati in futuri handshake TLS. Nuovi elenchi di priorità curva diventano attivi al successivo riavvio dopo aver ricevuto le impostazioni dei criteri.     

![GPP distribuire curve](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 Gestione TLS curva priorità tramite criteri di gruppo*


