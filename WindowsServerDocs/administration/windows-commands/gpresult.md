---
title: gpresult
description: Argomento di riferimento per il comando Gpresult, che visualizza le informazioni del gruppo di criteri risultante (RSoP) per un utente e un computer remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e88a75a15168baaf2e49ca08ff20d3a8ffb5620c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818861"
---
# <a name="gpresult"></a>gpresult

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le informazioni gruppo di criteri risultante (RSoP) per un utente remoto e il computer. Per utilizzare il reporting RSoP per computer di destinazione in modalità remota attraverso il firewall, è necessario disporre di regole firewall che consentono il traffico di rete in entrata sulle porte.

## <a name="syntax"></a>Sintassi

```
gpresult [/s <system> [/u <username> [/p [<password>]]]] [/user [<targetdomain>\]<targetuser>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <filename> [/f] | /?}
```

> [!NOTE]
> Eccetto quando si usa **/?**, è necessario includere un'opzione di output, **/r**, **/v**, **/z**, **/x**o **/h**.

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /s`<system>` | Specifica il nome o indirizzo IP di un computer remoto. Non usare barre rovesciate. Il valore predefinito è il computer locale. |
| /u`<username>` | Utilizza le credenziali dell'utente specificato per eseguire il comando. L'utente predefinito è l'utente connesso al computer che esegue il comando. |
| /p`[<password>]` | Specifica la password dell'account utente fornito nel **/u** parametro. Se **/p** viene omesso, **gpresult** richiesta la password. Il **/p** parametro non può essere usato con **/x** o **/h**. |
| /User`[<targetdomain>\]<targetuser>]` | Specifica l'utente remoto i cui dati risultante vanno visualizzato. |
| /Scope`{user | computer}` | Visualizza i dati di gruppo di criteri risultante per l'utente o il computer. Se **/ambito** viene omesso, **gpresult** sono riportati i dati gruppo di criteri risultante per l'utente sia il computer. |
| `[/x | /h] <filename>` | Salva il report in formato XML (**/x**) o HTML (**/h**) nella posizione e con il nome file specificato dal parametro *filename* . Non può essere usato con **/u**, **/p**, **/r**, **/v**o **/z**. |
| /f | Forza **gpresult** per sovrascrivere il nome del file specificato nella **/x** o **/h** (opzione). |
| /r | Consente di visualizzare i dati di riepilogo risultante. |
| /v | Consente di visualizzare informazioni dettagliate sui criteri. Ciò include le impostazioni dettagliate che sono state applicate con una priorità pari a 1. |
| /z | Visualizza tutte le informazioni disponibili sui criteri di gruppo. Ciò include le impostazioni dettagliate che sono state applicate con una precedenza di 1 e versioni successive. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Criteri di gruppo sono lo strumento di amministrazione principale per definire e controllare il funzionamento dei programmi, risorse di rete e il sistema operativo per utenti e computer di un'organizzazione. In un ambiente Active Directory, Criteri di gruppo viene applicato a utenti o computer in base all'appartenenza a siti, domini o unità organizzative.

- Poiché è possibile applicare le impostazioni dei criteri sovrapposti a qualsiasi computer o utente, la funzionalità criteri di gruppo genera un set di impostazioni di criteri risultante quando l'utente effettua l'accesso. Il comando **Gpresult** Visualizza il set di impostazioni dei criteri risultante applicato nel computer per l'utente specificato quando l'utente ha effettuato l'accesso.

- Poiché **/v** e **/z** producono una grande quantità di informazioni, è utile reindirizzare l'output a un file di testo (ad esempio, `gpresult/z >policy.txt` ).

### <a name="examples"></a>Esempi

Per recuperare i dati RSoP solo per l'utente remoto, *maindom\hiropln* con la password *p@ssW23* , che si trova nel computer *SRVPRINC*, digitare:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```

Per salvare tutte le informazioni disponibili su Criteri di gruppo in un file denominato, *policy. txt*, solo per l'utente remoto *maindom\hiropln* con la password *p@ssW23* , nel computer *SRVPRINC*, digitare:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```

Per visualizzare i dati RSoP per l'utente connesso, *maindom\hiropln* con la password *p@ssW23* per il computer *SRVPRINC*, digitare:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
