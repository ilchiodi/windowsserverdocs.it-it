---
title: tpmvscmgr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6cfc15b7c089c9b6ad4d9a267b951373e63a63a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888592"
---
# <a name="tpmvscmgr"></a>tpmvscmgr



Lo strumento da riga di comando Tpmvscmgr consente agli utenti con credenziali amministrative creare ed eliminare smart card virtuali TPM in un computer. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

### <a name="parameters-for-create-command"></a>Parametri per il comando Create

Il comando Create imposta una nuova smart card virtuali nel sistema dell'utente. Restituisce l'ID dell'istanza della scheda appena creata per riferimento futuro se è necessaria l'eliminazione. L'istanza nel formato ID **ROOT\SMARTCARDREADER\000n** dove **n** inizia da 0 e aumenta di 1 ogni volta che si crea una nuova smart card virtuale.

|Parametro|Descrizione|
|---------|-----------|
|al nome|Obbligatorio. Indica il nome della nuova smart card virtuale.|
|/ AdminKey|Indica la chiave amministratore desiderato che può essere utilizzata per reimpostare il PIN della scheda se un utente dimentica il PIN.</br>**PREDEFINITO** Specifica il valore predefinito di 010203040506070801020304050607080102030405060708.</br>**Prompt dei COMANDI** richiede all'utente di immettere un valore per la chiave amministratore.</br>**CASUALE** comporta un'impostazione casuale per la chiave di amministrazione per una smart card che non viene restituita all'utente. Verrà creata una smart card che potrebbe non essere gestita tramite strumenti di gestione delle smart card. Quando è stato generato con RANDOM, la chiave amministratore deve essere inserita come 48 caratteri esadecimali.|
|/ PIN|Indica il valore PIN utente desiderato.</br>**PREDEFINITO** Specifica il valore predefinito di 12345678 PIN.</br>**Prompt dei COMANDI** richiede all'utente di immettere un PIN nella riga di comando. Il PIN deve contenere un minimo di otto caratteri e può contenere numeri, caratteri e caratteri speciali.|
|/ PUK|Indica il valore di chiave PIN sblocco (PUK) desiderato. Il valore PUK deve essere un minimo di otto caratteri e può contenere numeri, caratteri e caratteri speciali. Se il parametro viene omesso, la scheda viene creata senza un PUK.</br>**PREDEFINITO** Specifica il valore predefinito di 12345678 PUK.</br>**Prompt dei COMANDI** chiede all'utente di immettere un PUK nella riga di comando.|
|/ generare|Genera i file nell'archiviazione necessari per la smart card virtuale alla funzione. Se il / generare parametro viene omesso, è equivalente alla creazione di una scheda senza questo file system. Una scheda senza un file system può essere gestita solo da un sistema di gestione delle smart card, ad esempio Microsoft Configuration Manager.|
|/Machine|Consente di specificare il nome di un computer remoto in cui è possibile creare la smart card virtuale. Può essere utilizzato in un solo ambiente di dominio e si basa su DCOM. Per il comando abbia esito positivo per la creazione di una smart card virtuale in un computer diverso, l'utente che esegue questo comando deve essere un membro del gruppo administrators locale nel computer remoto.|
|/?|Visualizza la Guida per questo comando.|

### <a name="parameters-for-destroy-command"></a>Parametri del comando di eliminazione

Il comando di eliminazione Elimina in modo sicuro una smart card virtuale dal computer dell'utente.

> [!WARNING]
> Quando viene eliminata una smart card virtuale, non può essere recuperato.

|Parametro|Descrizione|
|---------|-----------|
|/istanza|Specifica l'ID di istanza di smart card virtuale da rimuovere. L'ID istanza generato come output da Tpmvscmgr.exe quando è stata creata la scheda. Il parametro /istanza è un campo obbligatorio per il comando di eliminazione.|
|/?|Visualizza la Guida per questo comando.|

## <a name="remarks"></a>Note

L'appartenenza di **amministratori** gruppo (o equivalente) nel computer di destinazione è il requisito minimo necessario per eseguire tutti i parametri del comando.

Per gli input alfanumerico, è consentito il set di ASCII completo 127 caratteri.

## <a name="BKMK_Examples"></a>Esempi

Il comando seguente viene illustrato come creare una smart card virtuale che può essere gestita in un secondo momento da uno strumento di gestione delle smart card avviato da un altro computer.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey DEFAULT /PIN PROMPT
```
In alternativa, anziché utilizzare una chiave amministratore predefinito, è possibile creare una chiave amministratore nella riga di comando. Il comando seguente viene illustrato come creare una chiave amministratore.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey PROMPT /PIN PROMPT
```
Il comando seguente creerà il non gestita smart card virtuale che può essere utilizzata per registrare i certificati.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey RANDOM /PIN PROMPT /generate
```
Il comando seguente crea una smart card virtuale con una chiave amministratore casuale. La chiave viene automaticamente eliminata dopo il cardis creato. Ciò significa che se l'utente dimentica il PIN o richiede la modifica del PIN, l'utente deve eliminare la scheda e crearne uno nuovo. Per eliminare la scheda, l'utente può eseguire il comando seguente.
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
in cui \<ID istanza > è il valore stampato sullo schermo quando l'utente ha creato la scheda. In particolare, per la prima scheda creata, l'ID istanza è ROOT\SMARTCARDREADER\0000.

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)