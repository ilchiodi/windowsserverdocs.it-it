---
title: Comandi netsh per la rete Mobile Broadband (MBN)
description: Usa netsh mbn per eseguire query e configurare parametri e impostazioni di Mobile Broadband.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
author: apdutta
ms.date: 02/20/2020
ms.openlocfilehash: 478f87db4d520a133b3d70c0ed2dbb4e91db60d9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853734"
---
# <a name="netsh-mbn-commands"></a>Comandi netsh mbn


Usa **netsh mbn** per eseguire query e configurare parametri e impostazioni di Mobile Broadband.

> [!TIP]
> Per informazioni sul comando netsh mbn, puoi usare 
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh mbn /?

Per netsh mbn sono disponibili i comandi seguenti:

- [add](#add)
- [connect](#connect)
- [delete](#delete)
- [disconnect](#disconnect)
- [diagnose](#diagnose)
- [dump](#dump)
- [help](#help)
- [set](#set)
- [show](#show)

## <a name="add"></a>aggiungi

Aggiunge una voce di configurazione a una tabella.

Per netsh mbn add sono disponibili i comandi seguenti:

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

Aggiunge un profilo di configurazione DM nell'archivio dati profili.

**Sintassi**

```powershell
add dmprofile [interface=]<string> [name=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **name**      | Nome del file XML del profilo. È il nome del file XML contenente i dati del profilo.     | Obbligatoria |


**Esempio**

```powershell
add dmprofile interface="Cellular" name="Profile1.xml"
```


### <a name="profile"></a>profile

Aggiunge un profilo di rete nell'archivio dati profili.

**Sintassi**

```powershell
add profile [interface=]<string> [name=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **name**      | Nome del file XML del profilo. È il nome del file XML contenente i dati del profilo.     | Obbligatoria |


**Esempio**

```powershell
add profile interface="Cellular" name="Profile1.xml"
```


## <a name="connect"></a>connect

Esegue la connessione a una rete Mobile Broadband.

**Sintassi**

```powershell
connect [interface=]<string> [connmode=]tmp|name [name=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **connmode**  | Specifica come vengono resi disponibili i parametri di connessione. È possibile richiedere la connessione tramite un profilo XML o tramite il nome del profilo XML archiviato in precedenza nell'archivio dati profili di Mobile Broadband con il comando "netsh mbn add profile". Nel primo caso il parametro connmode conterrà "tmp" come valore, mentre nel secondo caso conterrà "name".                                       | Obbligatoria |
| **name**      | Nome del file XML del profilo. È il nome del file XML contenente i dati del profilo.     | Obbligatoria |


**Esempi**

```powershell
connect interface="Cellular" connmode=tmp name=d:\profile.xml
connect interface="Cellular" connmode=name name=MyProfileName
```


## <a name="delete"></a>Elimina

Elimina una voce di configurazione da una tabella.

Per netsh mbn delete sono disponibili i comandi seguenti:

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

Elimina un profilo di configurazione DM dall'archivio dati profili.

**Sintassi**

```powershell
delete dmprofile [interface=]<string> [name=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **name**      | Nome del file XML del profilo. È il nome del file XML contenente i dati del profilo.     | Obbligatoria |

**Esempio**

```powershell
delete dmprofile interface="Cellular" name="myProfileName"
```

### <a name="profile"></a>profile

Elimina un profilo di rete dall'archivio dati profili.

**Sintassi**

```powershell
delete profile [interface=]<string> [name=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **name**      | Nome del file XML del profilo. È il nome del file XML contenente i dati del profilo.     | Obbligatoria |


**Esempio**

```powershell
delete profile interface="Cellular" name="myProfileName"
```

## <a name="diagnose"></a>diagnose

Esegue la diagnostica relativa ai problemi di base della rete dati.

**Sintassi**

```powershell
diagnose [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
diagnose interface="Cellular"
```


## <a name="disconnect"></a>disconnessione

Esegue la disconnessione da una rete Mobile Broadband.

**Sintassi**

```powershell
disconnect [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
disconnect interface="Cellular"
```


## <a name="dump"></a>dump

Visualizza uno script di configurazione. 

Crea uno script contenente la configurazione corrente.  Se viene salvato in un file, lo script può essere usato per ripristinare le impostazioni di configurazione modificate.

**Sintassi**

```powershell
dump
```

## <a name="help"></a>help

Visualizza un elenco di comandi.

**Sintassi**

```powershell
help
```

## <a name="set"></a>set

Imposta le informazioni di configurazione.

Per netsh mbn set sono disponibili i comandi seguenti:

- [acstate](#acstate)
- [dataenablement](#dataenablement)
- [dataroamcontrol](#dataroamcontrol)
- [enterpriseapnparams](#enterpriseapnparams)
- [highestconncategory](#highestconncategory)
- [powerstate](#powerstate)
- [profileparameter](#profileparameter)
- [slotmapping](#slotmapping)
- [tracing](#tracing)

### <a name="acstate"></a>acstate

Imposta lo stato di connessione automatica dei dati Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
set acstate [interface=]<string> [state=]autooff|autoon|manualoff|manualon
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **name**      | Stato di connessione automatica da impostare. Uno dei valori seguenti:<br>autooff: token di connessione automatica disattivato.<br>autoon: token di connessione automatica attivato.<br>manualoff: token di connessione manuale disattivato.<br>manualon: token di connessione manuale attivato. | Obbligatoria |


**Esempio**

```powershell
set acstate interface="Cellular" state=autoon
```


### <a name="dataenablement"></a>dataenablement

Attiva o disattiva i dati Mobile Broadband per il set di profili e l'interfaccia specificati.

**Sintassi**

```powershell
set dataenablement [interface=]<string> [profileset=]internet|mms|all [mode=]yes|no
```

**Parametri**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **profileset** | Nome del set di profili.                                                                      | Obbligatoria |
| **mode**       | Uno dei valori seguenti:<br>yes: abilita il set di profili di destinazione.<br>no: disabilita il set di profili di destinazione.| Obbligatoria |


**Esempio**

```powershell
set dataenablement interface="Cellular" profileset=mms mode=yes
```


### <a name="dataroamcontrol"></a>dataroamcontrol

Imposta lo stato del controllo di roaming dei dati Mobile Broadband per il set di profili e l'interfaccia specificati.

**Sintassi**

```powershell
set dataroamcontrol [interface=]<string> [profileset=]internet|mms|all [state=]none|partner|all
```

**Parametri**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **profileset** | Nome del set di profili.                                                                      | Obbligatoria |
| **mode**       | Uno dei valori seguenti:<br>none: solo operatore domestico.<br>partner: solo operatori domestico e partner.<br>all: operatori domestico, partner e di roaming.| Obbligatoria |


**Esempio**

```powershell
set dataroamcontrol interface="Cellular" profileset=mms mode=partner
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

Imposta i parametri enterpriseAPN dei dati Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
set enterpriseapnparams [interface=]<string> [allowusercontrol=]yes|no|nc [allowuserview=]yes|no|nc [profileaction=]add|delete|modify|nc
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **allowusercontrol** | Uno dei valori seguenti:<br>yes: consente all'utente finale di controllare enterpriseAPN.<br>no: non consente all'utente finale di controllare enterpriseAPN.<br>nc: nessuna modifica. | Obbligatoria |
| **allowuserview** |Uno dei valori seguenti:<br>yes: consente all'utente finale di visualizzare enterpriseAPN.<br>no: non consente all'utente finale di visualizzare enterpriseAPN.<br>nc: nessuna modifica. | Obbligatoria |
| **profileaction** | Uno dei valori seguenti:<br>add: un profilo enterpriseAPN è stato aggiunto.<br>delete: un profilo enterpriseAPN è stato eliminato.<br>modify: un profilo enterpriseAPN è stato modificato.<br>nc: nessuna modifica. | Obbligatoria |


**Esempio**

```powershell
set enterpriseapnparams interface="Cellular" profileset=mms mode=yes
```


### <a name="highestconncategory"></a>highestconncategory

Imposta la più alta categoria di connessione dei dati Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
set highestconncategory [interface=]<string> [highestcc=]admim|user|operator|device
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **highestcc** | Uno dei valori seguenti:<br>admin: profili con provisioning da parte dell'amministratore.<br>user: profili con provisioning da parte dell'utente.<br>operator: profili con provisioning da parte dell'operatore.<br>device: profili con provisioning da parte del dispositivo. | Obbligatoria |


**Esempio**

```powershell
set highestconncategory interface="Cellular" highestcc=operator
```


### <a name="powerstate"></a>powerstate

Attiva o disattiva la radio Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
set powerstate [interface=]<string> [state=]on|off
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **state**      | Uno dei valori seguenti:<br>on: accende il ricetrasmettitore radio.<br>off:  spegne il ricetrasmettitore radio. | Obbligatoria |


**Esempio**

```powershell
set powerstate interface="Cellular" state=on
```


### <a name="profileparameter"></a>profileparameter

Imposta i parametri in un profilo di rete Mobile Broadband.

**Sintassi**

```powershell
set profileparameter [name=]<string> [[interface=]<string>] [[cost]=default|unrestricted|fixed|variable]
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nome del profilo da modificare. Se è specificata l'interfaccia, verrà modificato solo il profilo presente in tale interfaccia. | Obbligatoria |
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Facoltativo |
| **cost**      | Costo associato al profilo.                                                             | Facoltativo |


**Note**

È necessario specificare almeno un parametro tra interface e cost.


**Esempio**

```powershell
set profileparameter name="profile 1" cost=default
```


### <a name="slotmapping"></a>slotmapping

Imposta il mapping dello slot del modem Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
set slotmapping [interface=]<string> [slotindex=]<integer>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **slotindex** | Indice dello slot da impostare.                                                                         | Obbligatoria |


**Esempio**

```powershell
set slotmapping interface="Cellular" slotindex=1
```


### <a name="tracing"></a>tracing

Abilita o disabilita la traccia.

**Sintassi**

```powershell
set tracing [mode=]yes|no
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **mode**      | Uno dei valori seguenti:<br>yes: abilita la traccia per Mobile Broadband.<br>no: disabilita la traccia per Mobile Broadband.     | Obbligatoria |


**Esempio**

```powershell
set tracing mode=yes
```

## <a name="show"></a>show

Visualizza le informazioni della rete Mobile Broadband.

Per netsh mbn set sono disponibili i comandi seguenti:

- [acstate](#acstate)
- [capability](#capability)
- [connection](#connection)
- [dataenablement](#dataenablement)
- [dataroamcontrol](#dataroamcontrol)
- [dmprofiles](#dmprofiles)
- [enterpriseapnparams](#enterpriseapnparams)
- [highestconncategory](#highestconncategory)
- [homeprovider](#homeprovider)
- [interfaces](#interfaces)
- [netlteattachinfo](#netlteattachinfo)
- [pin](#pin)
- [pinlist](#pinlist)
- [preferredproviders](#preferredproviders)
- [profiles](#profiles)
- [profilestate](#profilestate)
- [provisionedcontexts](#provisionedcontexts)
- [purpose](#purpose)
- [radio](#radio)
- [readyinfo](#readyinfo)
- [signal](#signal)
- [slotmapping](#slotmapping)
- [slotstatus](#slotstatus)
- [smsconfig](#smsconfig)
- [tracing](#tracing)
- [visibleproviders](#visibleproviders)

### <a name="acstate"></a>acstate  

Visualizza lo stato di connessione automatica dei dati Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show acstate [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show acstate interface="Cellular"
```


### <a name="capability"></a>capability

Visualizza le informazioni sulle funzionalità dell'interfaccia specificata.

**Sintassi**

```powershell
show capability [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show capability interface="Cellular"
```


### <a name="connection"></a>connection

Visualizza le informazioni sulla connessione corrente per l'interfaccia specificata.

**Sintassi**

```powershell
show connection [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show connection interface="Cellular"
```


### <a name="dataenablement"></a>dataenablement

Visualizza lo stato di abilitazione dei dati Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show dataenablement [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show dataenablement interface="Cellular"
```


### <a name="dataroamcontrol"></a>dataroamcontrol

Visualizza lo stato del controllo di roaming dei dati Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show dataroamcontrol [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show dataroamcontrol interface="Cellular"
```


### <a name="dmprofiles"></a>dmprofiles

Visualizza l'elenco dei profili di configurazione DM configurati nel sistema.

**Sintassi**

```powershell
show dmprofiles [[name=]<string>] [[interface=]<string>]
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nome del profilo da visualizzare.                                                               | Facoltativo |
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Facoltativo |

**Note**
    
Visualizza i dati del profilo o elenca i profili presenti nel sistema.

Se è specificato il nome del profilo, verrà visualizzato il contenuto del profilo. In caso contrario, verrà visualizzato l'elenco dei profili per l'interfaccia.

Se è specificato il nome dell'interfaccia, verrà visualizzato solo il profilo specificato nell'interfaccia specificata. In caso contrario, verrà visualizzato il primo profilo corrispondente.

**Esempio**

```powershell
show dmprofiles name="profile 1" interface="Cellular"
show dmprofiles name="profile 2"
show dmprofiles
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

Visualizza i parametri enterpriseAPN dei dati Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show enterpriseapnparams [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show enterpriseapnparams interface="Cellular"
```


### <a name="highestconncategory"></a>highestconncategory

Visualizza la più alta categoria di connessione dei dati Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show highestconncategory [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show highestconncategory interface="Cellular"
```


### <a name="homeprovider"></a>homeprovider

Visualizza le informazioni sul provider principale per l'interfaccia specificata.

**Sintassi**

```powershell
show homeprovider [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show homeprovider interface="Cellular"
```


### <a name="interfaces"></a>interfaces

Visualizza l'elenco di interfacce Mobile Broadband presenti nel sistema. Per questo comando non è disponibile alcun parametro.

**Sintassi**

```powershell
show interfaces
```


### <a name="netlteattachinfo"></a>netlteattachinfo

Visualizza le informazioni di collegamento LTE della rete Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show netlteattachinfo [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show netlteattachinfo interface="Cellular"
```

### <a name="pin"></a>aggiungi      

Visualizza le informazioni sul PIN per l'interfaccia specificata.

**Sintassi**

```powershell
show pin [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show pin interface="Cellular"
```


### <a name="pinlist"></a>pinlist  

Visualizza le informazioni sull'elenco di PIN per l'interfaccia specificata.

**Sintassi**

```powershell
show pinlist [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show pinlist interface="Cellular"
```


### <a name="preferredproviders"></a>preferredproviders

Visualizza l'elenco dei provider preferiti per l'interfaccia specificata.

**Sintassi**

```powershell
show preferredproviders [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show preferredproviders interface="Cellular"
```


### <a name="profiles"></a>profiles 

Visualizza l'elenco dei profili configurati nel sistema.

**Sintassi**

```powershell
show profiles [[name=]<string>] [[interface=]<string>] [[purpose=]<string>]
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nome del profilo da visualizzare.                                                               | Facoltativo |
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Facoltativo |
| **purpose**   | Scopo | Facoltativo |

**Note**

Se è specificato il nome del profilo, verrà visualizzato il contenuto del profilo. In caso contrario, verrà visualizzato l'elenco dei profili per l'interfaccia.

Se è specificato il nome dell'interfaccia, verrà visualizzato solo il profilo specificato nell'interfaccia specificata. In caso contrario, verrà visualizzato il primo profilo corrispondente.

Se è specificato lo scopo, verranno visualizzati solo i profili con il corrispondente GUID dello scopo.  In caso contrario, i profili non verranno filtrati in base allo scopo.  La stringa può essere un GUID tra parentesi graffe o una delle stringhe seguenti: internet, supl, mms, ims o allhost.
    
**Esempio**

```powershell
show profiles interface="Cellular" purpose="{00000000-0000-0000-0000-000000000000}"
show profiles name="profile 1" interface="Cellular"
show profiles name="profile 2"
show profiles
```

### <a name="profilestate"></a>profilestate

Visualizza lo stato di un profilo Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show profilestate [interface=]<string> [name=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |
| **name**      | Nome del profilo. È il nome del profilo con stato da visualizzare.            | Obbligatoria |

**Esempio**

```powershell
show profilestate interface="Cellular" name="myProfileName"
```

### <a name="provisionedcontexts"></a>provisionedcontexts

Visualizza le informazioni sui contesti di cui è stato effettuato il provisioning per l'interfaccia specificata.

**Sintassi**

```powershell
show provisionedcontexts [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show provisionedcontexts interface="Cellular"
```


### <a name="purpose"></a>purpose  

Visualizza i GUID dei gruppi di scopi che possono essere usati per filtrare i profili nel dispositivo. Per questo comando non è disponibile alcun parametro.

**Sintassi**

```powershell
show purpose
```


### <a name="radio"></a>radio    

Visualizza le informazioni sullo stato della radio per l'interfaccia specificata.

**Sintassi**

```powershell
show radio [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show radio interface="Cellular"
```


### <a name="readyinfo"></a>readyinfo

Visualizza le informazioni relative alla disponibilità dell'interfaccia specificata.

**Sintassi**

```powershell
show readyinfo [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show readyinfo interface="Cellular"
```


### <a name="signal"></a>signal   

Visualizza le informazioni sul segnale per l'interfaccia specificata.

**Sintassi**

```powershell
show signal [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show signal interface="Cellular"
```


### <a name="slotmapping"></a>slotmapping

Visualizza il mapping dello slot del modem Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show slotmapping [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show slotmapping interface="Cellular"
```


### <a name="slotstatus"></a>slotstatus

Visualizza lo stato dello slot del modem Mobile Broadband per l'interfaccia specificata.

**Sintassi**

```powershell
show slotstatus [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show slotstatus interface="Cellular"
```


### <a name="smsconfig"></a>smsconfig

Visualizza le informazioni sulla configurazione SMS per l'interfaccia specificata.

**Sintassi**

```powershell
show smsconfig [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show smsconfig interface="Cellular"
```


### <a name="tracing"></a>tracing  

Mostra se la traccia Mobile Broadband è abilitata o disabilitata.

**Sintassi**

```powershell
show tracing 
```


### <a name="visibleproviders"></a>visibleproviders

Visualizza l'elenco dei provider visibili per l'interfaccia specificata.

**Sintassi**

```powershell
show visibleproviders [interface=]<string>
```

**Parametri**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nome dell'interfaccia. È uno dei nomi di interfaccia visualizzati dal comando "netsh mbn show interfaces". | Obbligatoria |


**Esempio**

```powershell
show visibleproviders interface="Cellular"
```
