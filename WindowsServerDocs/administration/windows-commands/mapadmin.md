---
title: mapadmin
description: Argomento di riferimento per il comando mapadmin, che gestisce il mapping dei nomi utente per i servizi Microsoft per il file System di rete.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 059419a134b62ec92b30feacd086e7d7116aab25
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354651"
---
# <a name="mapadmin"></a>mapadmin

L'utilità da riga di comando **mapadmin** gestisce il mapping dei nomi utente nel computer locale o remoto che esegue i servizi Microsoft per il file System di rete. Se si è connessi con un account che non dispone di credenziali amministrative, è possibile specificare un nome utente e una password di un account che lo esegue.

## <a name="syntax"></a>Sintassi

```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <windowsuser> -uu <UNIXuser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <windowsgroup> -ug <UNIXgroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <Windowsuser> [-uu <UNIXuser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <Windowsgroup> [-ug <UNIXgroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <Windowsdomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <Windowsdomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<computer>` | Specifica il computer remoto che esegue il servizio di mapping dei nomi utente che si desidera amministrare. È possibile specificare il computer utilizzando un nome WINS (Windows Internet Name Service) o un nome di Domain Name System (DNS) o un indirizzo IP (Internet Protocol). |
| -u`<user>` | Specifica il nome utente dell'utente di cui si desidera utilizzare le credenziali. Potrebbe essere necessario aggiungere il nome di dominio al nome utente nel formato *DOMINIO\nomeutente*. |
| -p`<password>` | Specifica la password dell'utente. Se si specifica l'opzione **-u** omettendo l'opzione **-p** , verrà richiesta la password dell'utente. |
| `start | stop` | Avvia o arresta il servizio di mapping dei nomi utente. |
| config | Specifica le impostazioni generali per il mapping dei nomi utente. Con questo parametro sono disponibili le opzioni seguenti:<ul><li>**-r `<dddd>:<hh>:<mm>` :** specifica l'intervallo di aggiornamento per l'aggiornamento dai database Windows e NIS in giorni, ore e minuti. L'intervallo minimo è di 5 minuti.</li><li>**-i `{yes | no}` :** attiva il mapping semplice (**Sì**) o disattivato (**No**). Per impostazione predefinita, il mapping è attivato.</li></ul> |
| aggiungi | Crea un nuovo mapping per un utente o un gruppo. Con questo parametro sono disponibili le opzioni seguenti:<ul><li>**-Wu `<name>` :** specifica il nome dell'utente di Windows per il quale viene creato un nuovo mapping.</li><li>**-UU `<name>` :** specifica il nome dell'utente UNIX per il quale viene creato un nuovo mapping.</li><li>**-WG `<group>` :** specifica il nome del gruppo di Windows per il quale viene creato un nuovo mapping.</li><li>**-UG `<group>` :** specifica il nome del gruppo UNIX per il quale viene creato un nuovo mapping.</li><li>**-seprimary:** Specifica che il nuovo mapping è il mapping primario.</li></ul> |
| setprimary | Specifica il mapping primario per un utente o un gruppo UNIX con più mapping. Con questo parametro sono disponibili le opzioni seguenti:<ul><li>**-Wu `<name>` :** specifica l'utente di Windows del mapping primario. Se esiste più di un mapping per l'utente, utilizzare l'opzione **-UU** per specificare il mapping primario.</li><li>**-UU `<name>` :** specifica l'utente UNIX del mapping primario.</li><li>**-WG `<group>` :** specifica il gruppo di Windows del mapping primario. Se esiste più di un mapping per il gruppo, utilizzare l'opzione **-UG** per specificare il mapping primario.</li><li>**-UG `<group>` :** specifica il gruppo UNIX del mapping primario.</li></ul> |
| eliminare | Rimuove il mapping per un utente o un gruppo. Per questo parametro sono disponibili le opzioni seguenti:<ul><li>**-Wu `<user>` :** specifica l'utente di Windows per il quale verrà eliminato il mapping, specificato come `<windowsdomain>\<username>` .<p>È necessario specificare l'opzione **-Wu** o **-UU** oppure entrambe. Se si specificano entrambe le opzioni, il mapping specifico identificato dalle due opzioni verrà eliminato. Se si specifica solo l'opzione **-Wu** , verranno eliminati tutti i mapping per l'utente specificato.</li><li>**-UU `<user>` :** specifica l'utente UNIX per il quale verrà eliminato il mapping, specificato come `<username>` .<p>È necessario specificare l'opzione **-Wu** o **-UU** oppure entrambe. Se si specificano entrambe le opzioni, il mapping specifico identificato dalle due opzioni verrà eliminato. Se si specifica solo l'opzione **-UU** , verranno eliminati tutti i mapping per l'utente specificato.</li><li>**-WG `<group>` :** specifica il gruppo di Windows per il quale verrà eliminato il mapping, specificato come `<windowsdomain>\<username>` .<p>È necessario specificare l'opzione **-WG** o **-UG** oppure entrambe. Se si specificano entrambe le opzioni, il mapping specifico identificato dalle due opzioni verrà eliminato. Se si specifica solo l'opzione **-WG** , verranno eliminati tutti i mapping per il gruppo specificato.</li><li>**-UG `<group>` :** specifica il gruppo UNIX per il quale verrà eliminato il mapping, specificato come `<groupname>` .<p>È necessario specificare l'opzione **-WG** o **-UG** oppure entrambe. Se si specificano entrambe le opzioni, il mapping specifico identificato dalle due opzioni verrà eliminato. Se si specifica solo l'opzione **-UG** , verranno eliminati tutti i mapping per il gruppo specificato.</li></ul> |
| list | Visualizza informazioni sui mapping di utenti e gruppi. Con questo parametro sono disponibili le opzioni seguenti:<ul><li>**-tutti:** Elenca i mapping semplici e avanzati per utenti e gruppi.</li><li>**-semplice:** Elenca tutti gli utenti e i gruppi con mapping semplice.</li><li>**-Avanzate:** Elenca tutti gli utenti e i gruppi con mapping avanzati. Le mappe sono elencate nell'ordine in cui vengono valutate. Le mappe primarie, contrassegnate con un asterisco (*), vengono elencate per prime, seguite dalle mappe secondarie, contrassegnate con un carato `(^)` . </li> <li> * *-Wu `<name>` :** elenca il mapping per un utente di Windows specificato.</li><li>**-WG `<group>` :** elenca il mapping per un gruppo di Windows.</li><li>**-UU `<name>` :** elenca il mapping per un utente UNIX.</li><li>**-UG `<group>` :** elenca il mapping per un gruppo UNIX.</li></ul> |
| backup | Salva la configurazione del mapping dei nomi utente e i dati di mapping nel file specificato da `<filename>` . |
| ripristinare | Sostituisce i dati di configurazione e di mapping con i dati del file (specificato da `<filename>` ) creati utilizzando il parametro **backup** . |
| adddomainmap | Aggiunge una mappa semplice tra un dominio Windows e un dominio NIS o file di gruppo e di password. Per questo parametro sono disponibili le opzioni seguenti:<ul><li>**-d `<windowsdomain>` :** specifica il dominio di Windows di cui eseguire il mapping.</li><li>**-y `<NISdomain>` :** specifica il dominio NIS da mappare. È necessario utilizzare il parametro **- `<NISserver>` n** per specificare il server NIS per il dominio NIS specificato dall'opzione **-y** .</li><li>**-f `<path>` :** specifica il percorso completo della directory contenente i file di gruppo e password di cui eseguire il mapping. I file devono trovarsi nel computer gestito e non è possibile usare **mapadmin** per gestire un computer remoto per configurare le mappe in base ai file di password e di gruppo.</li></ul> |
| removedomainmap | Rimuove una mappa semplice tra un dominio Windows e un dominio NIS. Per questo parametro sono disponibili le opzioni e gli argomenti seguenti:<ul><li>**-d `<windowsdomain>` :** specifica il dominio di Windows della mappa da rimuovere.</li><li>**-y `<NISdomain>` :** specifica il dominio NIS della mappa da rimuovere.</li><li>**-tutti:** Specifica che devono essere rimosse tutte le mappe semplici tra i domini Windows e NIS. Verrà inoltre rimossa qualsiasi mappa semplice tra un dominio Windows e i file di gruppo e di password.</li></ul> |
| listdomainmaps | Elenca i domini di Windows di cui è stato eseguito il mapping a domini NIS o a file di gruppi e password. |

#### <a name="remarks"></a>Commenti

- Se non si specifica alcun parametri, il comando **mapadmin** Visualizza le impostazioni correnti per il mapping dei nomi utente.

- Per tutte le opzioni che specificano il nome di un utente o di un gruppo, è possibile usare i formati seguenti:

    - Per gli utenti di Windows, usare i formati: `<domain>\<username>` ,, `\\<computer>\<username>` `\<computer>\<username>` o`<computer>\<username>`

    - Per i gruppi di Windows, usare i formati seguenti: `<domain>\<groupname>` ,, `\\<computer>\<groupname>` `\<computer>\<groupname>` o`<computer>\<groupname>`

    - Per gli utenti UNIX, usare i formati: `<NISdomain>\<username>` ,, `<username>@<NISdomain>` `<username>@PCNFS` o`PCNFS\<username>`

    - Per i gruppi UNIX, usare i formati seguenti: `<NISdomain>\<groupname>` ,, `<groupname>@<NISdomain>` `<groupname>@PCNFS` o`PCNFS\<groupname>`

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
