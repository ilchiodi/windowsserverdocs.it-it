---
title: cipher
description: Argomento di riferimento per il comando cipher, che consente di visualizzare o modificare la crittografia delle directory e dei file nei volumi NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b07a0231b6cff6be9533136bf35ef013c854c2e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82713396"
---
# <a name="cipher"></a>cipher

Visualizza o modifica la crittografia delle directory e dei file in volumi NTFS. Se utilizzata senza parametri, **cipher** Visualizza lo stato di crittografia della directory corrente e qualsiasi file in esso contenuti.

## <a name="syntax"></a>Sintassi

```
cipher [/e | /d | /c] [/s:<directory>] [/b] [/h] [pathname [...]]
cipher /k
cipher /r:<filename> [/smartcard]
cipher /u [/n]
cipher /w:<directory>
cipher /x[:efsfile] [filename]
cipher /y
cipher /adduser [/certhash:<hash> | /certfile:<filename>] [/s:directory] [/b] [/h] [pathname [...]]
cipher /removeuser /certhash:<hash> [/s:<directory>] [/b] [/h] [<pathname> [...]]
cipher /rekey [pathname [...]]
```

### <a name="parameters"></a>Parametri

| Parametri | Descrizione |
| ---------- | ----------- |
| / b | Interrompe se viene rilevato un errore. Per impostazione predefinita, **cipher** continua a funzionare anche se vengono rilevati errori. |
| /C | Visualizza le informazioni sul file crittografato. |
| /d | Consente di decrittografare il file specificati o una directory. |
| /e | Crittografa il file specificati o una directory. Le directory vengono contrassegnate in modo che i file aggiunti successivamente verranno crittografati. |
| /h | Vengono visualizzati i file con nascosto o gli attributi di sistema. Per impostazione predefinita, questi file non crittografati o decrittografati. |
| /k | Crea un nuovo certificato e una chiave per l'utilizzo con i file Encrypting File System (EFS). Se il **/k** viene specificato, tutti gli altri parametri vengono ignorati. |
| r:`<filename>` [/smartcard] | Genera una chiave agente recupero dati EFS e un certificato, quindi li scrive in un file con estensione pfx (contenente certificato e chiave privata) e un file con estensione CER (contenente solo il certificato). Se **/smartcard** è specificato, scrive la chiave di ripristino e il certificato in una smart card e viene generato alcun file con estensione pfx. |
| /s`<directory>` | Esegue l'operazione specificata su tutte le sottodirectory nella *directory*specificata. |
| /u [/n] |  Trova tutti i file crittografati su unità locali. Se utilizzato con il **/n** parametro, non vengono effettuati aggiornamenti. Se utilizzato senza **/n**, **/u** Confronta chiave di crittografia dell'utente o la chiave dell'agente recupero dati a quelle esistenti e li aggiorna se sono stati modificati. Questo parametro funziona solo con **/n**. |
| /w`<directory>` | Rimuove i dati dallo spazio disponibile su disco inutilizzato dell'intero volume. Se si utilizza il **/w** parametro, tutti gli altri parametri vengono ignorati. La directory specificata può trovarsi in qualsiasi punto in un volume locale. Se è un montaggio punti o punti a una directory in un altro volume, i dati in tale volume viene rimossa. |
| /x [: efsfile] [`<FileName>`] | Esegue il backup del certificato EFS e chiavi per il nome file specificato. Se utilizzato con **: efsfile**, **/x** esegue il backup dei certificati dell'utente che sono stati utilizzati per crittografare il file. In caso contrario, l'utente corrente certificato EFS e le chiavi vengono eseguito il backup. |
| /y | Visualizza l'anteprima certificato EFS corrente nel computer locale. |
| /adduser [/certHash:`<hash>` | /CertFile:`<filename>`] |
| /rekey | Aggiorna i file crittografati specificati per utilizzare la chiave EFS attualmente configurata. |
| /certhash /RemoveUser:`<hash>` | Rimuove un utente dal file specificati. Il *Hash* previste **/certhash** deve essere l'hash SHA1 del certificato da rimuovere. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="remarks"></a>Osservazioni

- Se la directory padre non è crittografata, un file crittografato potrebbe diventare decrittografato quando viene modificata. Pertanto, quando si crittografa un file, è necessario crittografare la directory padre.

- Un amministratore può aggiungere il contenuto di un file con estensione CER per il criterio di recupero EFS per creare l'agente di ripristino per gli utenti e quindi importare il file con estensione pfx per ripristinare singoli file.

- È possibile utilizzare più nomi di directory e i caratteri jolly.

- È necessario inserire spazi tra più parametri.

## <a name="examples"></a>Esempi

Per visualizzare lo stato di crittografia di ognuno dei file e sottodirectory nella directory corrente, digitare:

```
cipher
```

I file e le directory crittografati sono contrassegnati con un **e**. I file e le directory non crittografati sono contrassegnati con una **U**. Ad esempio, l'output seguente indica che la directory corrente e tutto il relativo contenuto sono attualmente decrittografati:

```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```

Per abilitare la crittografia sulla directory privata utilizzata nell'esempio precedente, digitare:

```
cipher /e private
```

Verrà visualizzato il seguente output:

```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```

Il **cipher** comando Visualizza il seguente output:

```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```

Dove la directory **privata** è ora contrassegnata come crittografata.

### <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
