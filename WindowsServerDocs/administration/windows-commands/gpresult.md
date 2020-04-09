---
title: gpresult
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 480599a4040ab1fdcc3842cdb0eaa8c35afa873c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842464"
---
# <a name="gpresult"></a>gpresult

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le informazioni gruppo di criteri risultante (RSoP) per un utente remoto e il computer.
Per utilizzare il reporting RSoP per computer di destinazione in modalità remota attraverso il firewall, è necessario disporre di regole firewall che consentono il traffico di rete in entrata sulle porte.

## <a name="syntax"></a>Sintassi

```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```

### <a name="parameters"></a>Parametri

> [!NOTE]
> Tranne quando si utilizza **/?** , è necessario includere un'opzione di output, ovvero **/r**, **/v**, **/z**, **/x**, o **/h**.

|                Parametro                 |                                                                                                     Descrizione                                                                                                      |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /s \<sistema\>               |                                                  Specifica il nome o indirizzo IP di un computer remoto. Non utilizzare le barre rovesciate. Il valore predefinito è il computer locale.                                                   |
|             /u \<nome utente\>              |                                Utilizza le credenziali dell'utente specificato per eseguire il comando. L'utente predefinito è l'utente connesso al computer che esegue il comando.                                 |
|            /p [\<\>PASSWOrd]             |            Specifica la password dell'account utente fornito nel **/u** parametro. Se **/p** viene omesso, **gpresult** richiesta la password. **/p** non può essere utilizzato con **/x** o **/h**.            |
| /User [\<TARGETDOMAIN\>\\]\<TARGETUSER\> |                                                                            Specifica l'utente remoto i cui dati risultante vanno visualizzato.                                                                             |
|      /Scope {#124 & utente; computer}       |                                Visualizza i dati di gruppo di criteri risultante per l'utente o il computer. Se **/ambito** viene omesso, **gpresult** sono riportati i dati gruppo di criteri risultante per l'utente sia il computer.                                 |
|        [/x &#124; /h] <FILENAME>         | Salva il report in XML ( **/x**) o HTML ( **/h**) formato in corrispondenza della posizione e con il nome del file specificato dal *FILENAME* parametro. Non può essere utilizzato con **/u**, **/p**, **/r**, **/v**, o **/z**. |
|                    /f                    |                                                           impone a **Gpresult** di sovrascrivere il nome file specificato nell'opzione **/x** o **/h** .                                                           |
|                    /r                    |                                                                                             Consente di visualizzare i dati di riepilogo risultante.                                                                                              |
|                    /v                    |                                                    Consente di visualizzare informazioni dettagliate sui criteri. Ciò include le impostazioni dettagliate che sono state applicate con una priorità pari a 1.                                                    |
|                    /z                    |                                     Visualizza tutte le informazioni disponibili sui criteri di gruppo. Ciò include le impostazioni dettagliate che sono state applicate con una precedenza di 1 e versioni successive.                                      |
|                    /?                    |                                                                                         Visualizza la guida al prompt dei comandi.                                                                                         |

## <a name="remarks"></a>Note
- Criteri di gruppo sono lo strumento di amministrazione principale per definire e controllare il funzionamento dei programmi, risorse di rete e il sistema operativo per utenti e computer di un'organizzazione. In un ambiente Active Directory, Criteri di gruppo viene applicato a utenti o computer in base all'appartenenza a siti, domini o unità organizzative.
- Poiché è possibile applicare le impostazioni dei criteri sovrapposti a qualsiasi computer o utente, la funzionalità criteri di gruppo genera un set di impostazioni di criteri risultante quando l'utente effettua l'accesso. **Gpresult** Visualizza il set di impostazioni dei criteri risultante applicato nel computer per l'utente specificato quando l'utente ha effettuato l'accesso.
- Poiché **/v** e **/z** produrre grandi quantità di informazioni, è utile reindirizzare l'output in un file di testo (ad esempio, **gpresult/z > txt**).
- Il **gpresult** comando è disponibile in Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 e Windows Vista.
  ## <a name="examples"></a>Esempi
  Nell'esempio seguente recupera i dati di gruppo di criteri risultante per l'utente remoto **nome utente destinazione** del computer **Srvprinc**, e consente di visualizzare dati RSoP relativi solo all'utente. Il comando viene eseguito con le credenziali dell'utente **maindom\hiropln**e <strong>p@ssW23</strong> viene immesso come password per tale utente.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
  ```
  
Nell'esempio seguente consente di salvare tutte le informazioni disponibili sui criteri di gruppo per l'utente remoto **nome utente destinazione** del computer **Srvprinc** in un file denominato **txt**. Dati non sono inclusi sul computer. Il comando viene eseguito con le credenziali dell'utente **maindom\hiropln**e <strong>p@ssW23</strong> viene immesso come password per tale utente.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
  ```
  
Nell'esempio seguente sono riportati i dati gruppo di criteri risultante per il computer **Srvprinc** e l'utente connesso. Dati sono inclusi sull'utente e il computer. Il comando viene eseguito con le credenziali dell'utente **maindom\hiropln**e <strong>p@ssW23</strong> viene immesso come password per tale utente.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
  ```
  
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Criteri di gruppo TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)

- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
