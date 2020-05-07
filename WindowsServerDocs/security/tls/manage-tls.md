---
title: Gestire Transport Layer Security (TLS)
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 05/16/2018
ms.openlocfilehash: a11c41ad386618f85421ec46c6111791f468fa92
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850122"
---
# <a name="manage-transport-layer-security-tls"></a>Gestire Transport Layer Security (TLS)

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>Configurazione dell'ordine del pacchetto di crittografia TLS

Diverse versioni di Windows supportano i diversi pacchetti di crittografia TLS e l'ordine di priorità. Vedere pacchetti di [crittografia in TLS/SSL (SSP Schannel)](https://docs.microsoft.com/windows/win32/secauthn/cipher-suites-in-schannel) per l'ordine predefinito supportato dal provider Microsoft Schannel in diverse versioni di Windows.

> [!NOTE]
> È anche possibile modificare l'elenco di pacchetti di crittografia usando le funzioni CNG. per informazioni dettagliate, vedere priorità dei pacchetti di [crittografia Schannel](https://docs.microsoft.com/windows/win32/secauthn/prioritizing-schannel-cipher-suites) .

Le modifiche apportate all'ordine del pacchetto di crittografia TLS diverranno effettive al successivo avvio. Fino a quando non viene riavviato o arrestato, l'ordine esistente sarà attivo.

> [!WARNING]
> L'aggiornamento delle impostazioni del registro di sistema per l'ordine di priorità predefinito non è supportato e può essere reimpostato con gli aggiornamenti del servizio.

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configurazione dell'ordine del pacchetto di crittografia TLS tramite Criteri di gruppo

Per configurare l'ordine predefinito del pacchetto di crittografia TLS, è possibile usare le impostazioni di Criteri di gruppo dell'ordine dei pacchetti di crittografia SSL.

1. Dal console Gestione criteri di gruppo passare a **configurazione** > **computer modelli amministrativi** > **impostazioni di configurazione SSL**di**rete** > .
2. Fare doppio clic su **Order suite di crittografia SSL**, quindi fare clic sull'opzione **Enabled** .
3. Fare clic con il pulsante destro del mouse su **gruppi di crittografia SSL** e scegliere **Seleziona tutto** dal menu a comparsa.

   ![Impostazione di Criteri di gruppo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. Fare clic con il pulsante destro del mouse sul testo selezionato e scegliere **copia** dal menu a comparsa.
5. Incollare il testo in un editor di testo, ad esempio Notepad. exe, e aggiornare con il nuovo elenco di ordini del pacchetto di crittografia.

   > [!NOTE]
   > L'elenco di ordini del pacchetto di crittografia TLS deve essere in un formato delimitato da virgole. Ogni stringa del pacchetto di crittografia termina con una virgola (,) sul lato destro.
   >
   > Inoltre, l'elenco di pacchetti di crittografia è limitato a 1.023 caratteri.

6. Sostituire l'elenco nei pacchetti di **crittografia SSL** con l'elenco ordinato aggiornato.
7. Fare clic su **OK** o su **Applica**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configurazione dell'ordine del pacchetto di crittografia TLS tramite MDM

Il CSP dei criteri di Windows 10 supporta la configurazione dei pacchetti di crittografia TLS. Per ulteriori informazioni, vedere [Cryptography/TLSCipherSuites](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-cryptography#cryptography-tlsciphersuites) .

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurazione dell'ordine del pacchetto di crittografia TLS tramite i cmdlet di PowerShell per TLS

Il modulo di PowerShell TLS supporta il recupero dell'elenco ordinato dei pacchetti di crittografia TLS, la disabilitazione di un pacchetto di crittografia e l'abilitazione di un pacchetto di crittografia. Per ulteriori informazioni, vedere il [modulo TLS](https://docs.microsoft.com/powershell/module/tls/?view=win10-ps) .

## <a name="configuring-tls-ecc-curve-order"></a>Configurazione dell'ordine delle curve TLS ECC

A partire da Windows 10 & Windows Server 2016, l'ordine di curva ECC può essere configurato indipendentemente dall'ordine del pacchetto di crittografia. Se l'elenco di ordini del pacchetto di crittografia TLS presenta suffissi a curva ellittica, questi verranno sostituiti dal nuovo ordine di priorità della curva ellittica, se abilitato. Questo consente alle organizzazioni di usare un oggetto Criteri di gruppo per configurare versioni diverse di Windows con lo stesso ordine dei pacchetti di crittografia.

> [!NOTE]
> Prima di Windows 10, le stringhe del pacchetto di crittografia venivano aggiunte con la curva ellittica per determinare la priorità della curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Gestione delle curve ECC di Windows tramite CertUtil

A partire da Windows 10 e Windows Server 2016, Windows fornisce la gestione dei parametri a curva ellittica tramite l'utilità della riga di comando certutil. exe.
I parametri della curva ellittica vengono archiviati in bcryptprimitives. dll. Utilizzando certutil. exe, gli amministratori possono aggiungere e rimuovere parametri di curva rispettivamente da e verso Windows. Certutil. exe archivia i parametri della curva in modo sicuro nel registro di sistema.
Windows può iniziare a usare i parametri della curva in base al nome associato alla curva.

#### <a name="displaying-registered-curves"></a>Visualizzazione di curve registrate

Usare il comando certutil. exe seguente per visualizzare un elenco di curve registrate per il computer corrente.

```powershell
certutil.exe –displayEccCurve
```

![Curve di visualizzazione certutil](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figura 1. output di certutil. exe per visualizzare l'elenco delle curve registrate.*

#### <a name="adding-a-new-curve"></a>Aggiunta di una nuova curva

Le organizzazioni possono creare e usare i parametri della curva ricercati da altre entità attendibili.
Gli amministratori che desiderano utilizzare queste nuove curve in Windows devono aggiungere la curva.
Usare il comando certutil. exe seguente per aggiungere una curva al computer corrente:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- L'argomento **curvename** rappresenta il nome della curva in cui sono stati aggiunti i parametri della curva.
- L'argomento **curveParameters** rappresenta il nome file di un certificato che contiene i parametri delle curve che si desidera aggiungere.
- L'argomento **curveOid** rappresenta un nome di file di un certificato contenente l'OID dei parametri della curva che si desidera aggiungere (facoltativo).
- L'argomento **curveType** rappresenta un valore decimale della curva denominata dal [Registro di sistema della curva EC denominata](https://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (facoltativo).

![Certutil aggiungere curve](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 aggiunta di una curva tramite Certutil. exe.*

#### <a name="removing-a-previously-added-curve"></a>Rimozione di una curva precedentemente aggiunta

Gli amministratori possono rimuovere una curva aggiunta in precedenza usando il comando certutil. exe seguente:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows non può utilizzare una curva denominata dopo che un amministratore ha rimosso la curva dal computer.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Gestione delle curve ECC di Windows con Criteri di gruppo

Le organizzazioni possono distribuire i parametri della curva al computer aziendale, aggiunto a un dominio, usando Criteri di gruppo e l'estensione del registro di sistema delle preferenze Criteri di gruppo.
Il processo per la distribuzione di una curva è:

1. In Windows 10 e Windows Server 2016, utilizzare **certutil. exe** per aggiungere una nuova curva denominata registrata a Windows.
2. Dallo stesso computer aprire il Console Gestione Criteri di gruppo (GPMC), creare un nuovo oggetto Criteri di gruppo e modificarlo.
3. Passare a **Configurazione computer | Preferenze | Impostazioni di Windows | Registro di sistema**.  Fare clic con il pulsante destro del mouse su **Registry**. Passare il puntatore su **nuovo** e selezionare **elemento della raccolta**. Rinominare l'elemento della raccolta in modo che corrisponda al nome della curva. Verrà creato un elemento della raccolta del registro di sistema per ogni chiave del registro di sistema in *HKEY_LOCAL_MACHINE \currentcontrolset\control\cryptography\eccparameters*.
4. Configurare la raccolta del registro di sistema per le preferenze di Criteri di gruppo appena creata aggiungendo un nuovo **elemento del registro** di sistema per ogni valore del registro di sistema elencato in *\[HKEY_LOCAL_MACHINE \currentcontrolset\control\cryptography\eccparameters curvename]*.
5. Distribuire l'oggetto Criteri di gruppo contenente Criteri di gruppo elemento della raccolta registro di sistema nei computer Windows 10 e Windows Server 2016 che devono ricevere le nuove curve denominate.

    ![GPP distribuisce le curve](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 uso delle preferenze di Criteri di gruppo per distribuire le curve*

## <a name="managing-tls-ecc-order"></a>Gestione dell'ordine di TLS

A partire da Windows 10 e Windows Server 2016, è possibile usare le impostazioni di criteri di gruppo dell'ordine di curva ECC. configurare l'ordine predefinito della curva TLS ECC.
Usando ECC generico e questa impostazione, le organizzazioni possono aggiungere al sistema operativo le proprie curve denominate attendibili (approvate per l'uso con TLS), quindi aggiungere le curve denominate alla priorità della curva Criteri di gruppo impostazione per assicurarsi che vengano usate nei futuri handshake TLS.
I nuovi elenchi priorità curva diventano attivi al riavvio successivo dopo aver ricevuto le impostazioni dei criteri.

![GPP distribuisce le curve](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 gestione della priorità della curva TLS con Criteri di gruppo*

