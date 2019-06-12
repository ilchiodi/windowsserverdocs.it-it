---
title: Gestire Transport Layer Security (TLS)
description: Sicurezza di Windows Server
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
ms.date: 05/16/2018
ms.openlocfilehash: 872647f09898bf8ae08ee69f28b717d28abf7c78
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447300"
---
# <a name="manage-transport-layer-security-tls"></a>Gestire Transport Layer Security (TLS)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>Configurazione TLS Cipher Suite ordine

Versioni diverse di Windows supportano diversi pacchetti di crittografia TLS e ordine di priorità. Visualizzare [pacchetti di crittografia in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) per l'ordine predefinito supportato dal Provider Schannel Microsoft nelle diverse versioni di Windows.

> [!NOTE] 
> È inoltre possibile modificare l'elenco dei pacchetti di crittografia usando funzioni di CNG, vedere [assegnare la priorità ai pacchetti di crittografia Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) per informazioni dettagliate.

Le modifiche apportate all'ordine TLS cipher suite saranno effettive all'avvio successivo. Fino al riavvio o l'arresto, l'ordine esistente resteranno attive.

> [!WARNING] 
> Aggiornamento delle impostazioni del Registro di sistema per l'ordine di priorità predefinito non è supportata e può essere reimpostato con gli aggiornamenti di manutenzione. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configurazione TLS Cipher Suite ordine tramite criteri di gruppo

È possibile utilizzare le impostazioni di criteri di gruppo dell'ordine di SSL Cipher Suite per configurare l'ordine predefinito TLS cipher suite.

1. Da Console Gestione criteri di gruppo, passare a **configurazione Computer** > **modelli amministrativi** > **reti**  >  **Le impostazioni di configurazione di SSL**.
2. Fare doppio clic su **ordine dei pacchetti di crittografia SSL**, quindi fare clic sui **abilitato** opzione.
3. Fare doppio clic su **pacchetti di crittografia SSL** e selezionare **Seleziona tutto** nel menu a comparsa.

   ![Impostazione di Criteri di gruppo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. Il testo selezionato e scegliere **copia** nel menu a comparsa.
5. Incollare il testo in un editor di testo, ad esempio notepad.exe e aggiornamento con il nuovo elenco di ordini cipher suite.

   > [!NOTE]
   > Elenco degli ordini TLS cipher suite deve essere in formato delimitato da virgole di tipo strict. Ogni stringa di crittografia suite terminerà con una virgola (,) sul lato destro di esso. 
   > 
   > Inoltre, l'elenco dei pacchetti di crittografia è limitato a 1.023 caratteri.

6. Sostituire l'elenco nel **pacchetti di crittografia SSL** con l'elenco ordinato aggiornato.
7. Fare clic su **OK** o **applicare**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configurazione TLS Cipher Suite ordine tramite MDM

Il provider CSP dei criteri di Windows 10 supporta la configurazione dei pacchetti di crittografia TLS. Visualizzare [Cryptography/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) per altre informazioni.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurazione TLS Cipher Suite ordine utilizzando i cmdlet di PowerShell di TLS

Il modulo PowerShell di TLS supporta il recupero dell'elenco ordinato dei pacchetti di crittografia TLS, la disabilitazione di un pacchetto di crittografia e l'abilitazione di un pacchetto di crittografia. Visualizzare [TLS modulo](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) per altre informazioni.

## <a name="configuring-tls-ecc-curve-order"></a>Configurazione di ordine di curva ECC TLS 

A partire da Windows 10 e Windows Server 2016, ordine curva ECC può essere configurato indipendentemente dall'ordine cipher suite. Se TLS cipher suite di ordinamento elenco suffissi a curva ellittica, si eseguirà l'override del nuovo ordine di priorità a curva ellittica, se abilitato. Ciò consente alle organizzazioni di usare un oggetto Criteri di gruppo per configurare diverse versioni di Windows con lo stesso ordine di pacchetti di crittografia.

> [!NOTE]
> Prima di Windows 10, pacchetti di crittografia suite stringhe sono stati aggiunti con la curva ellittica per determinare la priorità della curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>La gestione di curve ECC Windows utilizzando CertUtil

A partire da Windows 10 e Windows Server 2016, Windows prevede la gestione di parametro a curva ellittica tramite certutil.exe l'utilità della riga di comando. I parametri della curva ellittica vengono archiviati nel bcryptprimitives.dll. Usa certutil.exe, gli amministratori possono aggiungere e rimuovere i parametri della curva verso e da Windows, rispettivamente. Certutil.exe archivia i parametri della curva in modo sicuro nel Registro di sistema. Windows può iniziare a usare i parametri della curva per il nome associato della curva.    

#### <a name="displaying-registered-curves"></a>La visualizzazione delle curve registrate

Usare il comando certutil.exe seguente per visualizzare un elenco di curve registrato per il computer corrente.

```powershell
certutil.exe –displayEccCurve
```

![Curve di visualizzazione di certutil](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figura 1 Certutil.exe di output per visualizzare l'elenco delle curve registrate.*

#### <a name="adding-a-new-curve"></a>Aggiunta di una curva di nuovo

Le organizzazioni possono creare e usare i parametri della curva analizzati da altre entità attendibili.  
Gli amministratori che desiderano utilizzare queste curve di nuovo in Windows è necessario aggiungere la curva.  
Usare il comando certutil.exe seguente per aggiungere una curva a computer corrente:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- Il **curveName** argomento rappresenta il nome della curva in cui sono stati aggiunti i parametri della curva.
- Il **curveParameters** argomento rappresenta il nome del file di un certificato che contiene i parametri delle curve di cui si desidera aggiungere.
- Il **curveOid** argomento rappresenta il nome di un certificato che contiene l'OID dei parametri della curva si desidera aggiungere (facoltativo).
- Il **curveType** argomento rappresenta un valore decimale della curva denominata dalle [registro curva denominata EC](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (facoltativo).

![Certutil aggiungere curve](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 aggiungendo una curva con certutil.exe.*

#### <a name="removing-a-previously-added-curve"></a>Rimozione di una curva aggiunta in precedenza

Gli amministratori possono rimuovere una curva aggiunta in precedenza usando il comando certutil.exe seguente:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows non è possibile usare una curva denominata dopo che un amministratore rimuove la curva dal computer.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>La gestione delle curve ECC Windows tramite criteri di gruppo

Le organizzazioni possono distribuire i parametri della curva a enterprise, dominio, computer tramite criteri di gruppo e l'estensione del Registro di preferenze criteri di gruppo.  
Il processo per la distribuzione di una curva è:

1.  In Windows 10 e Windows Server 2016, usare **certutil.exe** per aggiungere una curva denominata registrata nuovi per Windows.
2.  Da quel computer stesso, aprire la Console Gestione criteri di gruppo (GPMC), creare un nuovo oggetto Criteri di gruppo e modificarlo.
3.  Passare a **configurazione Computer | Preferenze | Le impostazioni di Windows | Registro di sistema**.  Fare doppio clic su **Registro di sistema**. Passare il mouse su **New** e selezionare **elemento della raccolta**. Rinominare l'elemento della raccolta in base al nome della curva. Si creerà un elemento della raccolta del Registro di sistema per ogni chiave di registro *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurare la raccolta del Registro di sistema preferenza di criteri di gruppo appena creato aggiungendo una nuova **elemento Registro di sistema** per ogni valore del Registro di sistema elencata sotto *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ ECCParameters\[curveName]* .
5.  Distribuire l'oggetto Criteri di gruppo che contiene elementi di raccolta del Registro di sistema dei criteri di gruppo per computer Windows 10 e Windows Server 2016 che deve ricevere le curve denominate nuovo.

    ![GPP distribuire curve](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 Using Group Policy Preferences per distribuire le curve*

## <a name="managing-tls-ecc-order"></a>La gestione dell'ordine di TLS ECC

A partire da Windows 10 e Windows Server 2016, i criteri di gruppo dell'ordine di curva ECC impostazioni consentono di configurare il valore predefinito dell'ordine di curva ECC TLS. Usando ECC generico e questa impostazione, le organizzazioni possa aggiungere i propri denominato curve (che vengono approvate per l'uso con TLS) per il sistema operativo attendibile e quindi aggiungere le curve denominate per l'impostazione di criteri di gruppo di priorità curva per assicurarsi che vengono usati in futuro TLS Handshake. Nuovi elenchi con priorità curva diventano attivi al riavvio successivo dopo aver ricevuto le impostazioni dei criteri.     

![GPP distribuire curve](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 Gestione TLS curva priorità tramite criteri di gruppo*


