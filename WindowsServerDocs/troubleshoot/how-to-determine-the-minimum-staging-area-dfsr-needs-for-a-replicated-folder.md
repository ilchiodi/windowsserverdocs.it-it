---
title: Come determinare l'area di gestione temporanea minima DFSR esigenze per una cartella replicata
description: Questo articolo è una guida di riferimento rapido su come calcolare l'area di gestione temporanea minima necessaria per il corretto funzionamento di DFSR.
ms.prod: windows-server
ms.technology: server-general
ms.date: 06/10/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 5e5bfdbb90d2b3e631aaa020a173eca779f0d6b3
ms.sourcegitcommit: fa9a8badf4eb366aeeca7d2905e2cad711ee8dae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/11/2020
ms.locfileid: "84715005"
---
# <a name="how-to-determine-the-minimum-staging-area-dfsr-needs-for-a-replicated-folder"></a>Come determinare l'area di gestione temporanea minima DFSR esigenze per una cartella replicata

Questo articolo è una guida di riferimento rapido su come calcolare l'area di gestione temporanea minima necessaria per il corretto funzionamento di DFSR. I valori inferiori a questi possono causare un rallentamento della replica o l'arresto del tutto. 

Tenere presente che si tratta *solo dei minimi*. Quando si considerano le dimensioni dell'area di gestione temporanea, più grande sarà l'area di gestione temporanea, fino alla dimensione della cartella replicata. Vedere la sezione "come determinare se si ha un problema per l'area di gestione temporanea" e i post di Blog collegati alla fine di questo articolo per altre informazioni sui motivi per cui è importante avere un'area di gestione temporanea di dimensioni appropriate.

> [!Note]
> È inoltre presente un hotfix che consente di calcolare le dimensioni di gestione temporanea. [È disponibile l'aggiornamento per l'interfaccia di gestione Replica DFS (DFSR)](https://support.microsoft.com/kb/2607047)

## <a name="rules-of-thumb"></a>Regole empiriche

**Windows Server 2003 R2** : la quota dell'area di gestione temporanea deve essere di dimensioni maggiori dei 9 file più grandi nella cartella replicata

**Windows Server 2008 e 2008 R2** : la quota dell'area di gestione temporanea deve essere grande quanto i 32 file più grandi nella cartella replicata

La replica iniziale utilizzerà molto più tempo per l'area di gestione temporanea rispetto alla replica giornaliera. L'impostazione dell'area di gestione temporanea superiore al minimo durante la replica iniziale è fortemente consigliata se lo spazio su disco è disponibile

## <a name="where-do-i-get-powershell"></a>Dove si ottiene PowerShell?

PowerShell è incluso in Windows 2008 e versioni successive. È necessario installare PowerShell in Windows Server 2003. È possibile scaricare PowerShell per Windows 2003 [qui](https://support.microsoft.com/kb/968930).

## <a name="how-do-you-find-these-x-largest-files"></a>Come è possibile trovare questi X file più grandi?

Usare uno script di PowerShell per trovare i file 32 o 9 di dimensioni maggiori e determinare il numero di gigabyte che aggiungono (grazie a Ned Pyle per i comandi di PowerShell). In realtà verranno presentati tre script di PowerShell. Ognuno è utile per se stesso. Tuttavia, il numero 3 è il più utile.

1. Eseguire il comando seguente:  
   ```Powershell
   Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | ft name,length -wrap –auto
   ```
   
   Questo comando restituirà i nomi di file e le dimensioni dei file in byte. Utile se si desidera conoscere i file 32 di dimensioni maggiori nella cartella replicata, in modo da poter "visitare" i rispettivi proprietari.

2. Eseguire il comando seguente:  
   ```Poswershell
   Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | measure-object -property length –sum
   ```
   Questo comando restituirà il numero totale di byte dei file 32 di dimensioni maggiori nella cartella senza elencare i nomi dei file.

3. Eseguire il comando seguente:  
   ```Poswershell
   $big32 = Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | measure-object -property length –sum
   
   $big32.sum /1gb
   ```
   Questo comando otterrà il numero totale di byte di 32 file di dimensioni maggiori nella cartella e i calcoli matematici per convertire i byte in gigabyte. Questo comando è due righe separate. È possibile incollarli contemporaneamente nella shell dei comandi di PowerShell o eseguirli di nuovo.

## <a name="manual-walkthrough"></a>Procedura dettagliata manuale

Per illustrare il processo e sperare di approfondire la conoscenza di ciò che si sta facendo, si procederà manualmente in ogni parte.

L'esecuzione del comando 1 restituirà risultati simili all'output seguente. Questo esempio usa solo 16 file per brevità. Usare sempre 32 per i sistemi operativi Windows 2008 e versioni successive e 9 per Windows 2003 R2

### <a name="example-data-returned-by-powershell"></a>Dati di esempio restituiti da PowerShell

<table>
<tbody>
<tr class="odd">
<td>Nome</td>
<td>Length</td>
</tr>
<tr class="even">
<td><strong>File5.zip</strong></td>
<td>10286089216</td>
</tr>
<tr class="odd">
<td><strong>archive.zip</strong></td>
<td>6029853696</td>
</tr>
<tr class="even">
<td><strong>BACKUP.zip</strong></td>
<td>5751522304</td>
</tr>
<tr class="odd">
<td> <strong>file9.zip</strong></td>
<td>5472683008</td>
</tr>
<tr class="even">
<td><strong>MENTOS.zip</strong></td>
<td>5241586688</td>
</tr>
<tr class="odd">
<td><strong>File7.zip</strong></td>
<td>4321264640</td>
</tr>
<tr class="even">
<td><strong>file2.zip</strong></td>
<td>4176765952</td>
</tr>
<tr class="odd">
<td><strong>frd2.zip</strong></td>
<td>4176765952</td>
</tr>
<tr class="even">
<td><strong>BACKUP.zip</strong></td>
<td>4078994432</td>
</tr>
<tr class="odd">
<td><strong>File44.zip</strong></td>
<td>4058424320</td>
</tr>
<tr class="even">
<td><strong>file11.zip</strong></td>
<td>3858056192</td>
</tr>
<tr class="odd">
<td><strong>Backup2.zip</strong></td>
<td>3815138304</td>
</tr>
<tr class="even">
<td><strong>BACKUP3.zip</strong></td>
<td>3815138304</td>
</tr>
<tr class="odd">
<td><strong>Current.zip</strong></td>
<td>3576931328</td>
</tr>
<tr class="even">
<td><strong>Backup8.zip</strong></td>
<td>3307488256</td>
</tr>
<tr class="odd">
<td><strong>File999.zip</strong></td>
<td>3274982400</td>
</tr>
</tbody>
</table>

### <a name="how-to-use-this-data-to-determine-the-minimum-staging-area-size"></a>Come usare questi dati per determinare le dimensioni minime dell'area di gestione temporanea:

  - Nome = nome del file.
  - Lunghezza = byte
  - Un gigabyte = 1073741824 byte

In primo luogo, è necessario sommare il numero totale di byte. Dividere quindi il totale di 1073741824. Suggerisco di usare Excel o il foglio di calcolo scelto per eseguire la matematica.

### <a name="example"></a>Esempio

Dall'esempio sopra il numero totale di byte = 75241684992. Per ottenere la quota minima necessaria per l'area di gestione temporanea, è necessario dividere 75241684992 per 1073741824.

> 75241684992/1073741824 = 70,07 GB

In base a questi dati, l'area di gestione temporanea verrà impostata su 71 GB se arrotondato al numero intero più vicino.

### <a name="real-world-scenario"></a>Scenario reale:

Anche se una procedura dettagliata manuale è interessante, è probabile che non sia il momento migliore per eseguire il calcolo. Per automatizzare il processo, usare il comando 3 degli esempi precedenti. I risultati appariranno come segue

> [![immagine](https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/58/02/metablogapi/8204.image_thumb_02CB3914.png "image")](https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/58/02/metablogapi/0876.image_03A39EFE.png)

Usando il comando di esempio 3 senza alcuna ulteriore operazione, ad eccezione dell'arrotondamento al numero intero più vicino, posso determinare che ho bisogno di una quota di area di gestione temporanea di 6 GB per d: \\ docs.

## <a name="do-i-need-to-reboot-or-restart-the-service-for-the-changes-to-be-picked-up"></a>È necessario riavviare o riavviare il servizio per fare in modo che le modifiche vengano rilevate?

Per rendere effettive le modifiche apportate alla quota dell'area di gestione temporanea non è necessario riavviare o riavviare il servizio. Per applicare le modifiche, è necessario attendere la replica di Active Directory e il ciclo di polling di AD DFSR.

## <a name="how-to-determine-if-you-have-a-staging-area-problem"></a>Come determinare se si verifica un problema nell'area di gestione temporanea

È possibile rilevare i problemi dell'area di gestione temporanea monitorando gli ID eventi specifici nei server DFSR. L'elenco di eventi è 4202, 4204, 4206, 4208 e 4212. I testi di questi eventi sono elencati di seguito. È importante distinguere tra 4202 e 4204 e gli altri eventi. È possibile registrare un numero elevato di eventi 4202 e 4204 in condizioni operative normali. Gli eventi 4202 e 4204 possono essere considerati analoghi a quelli dell'impulso, mentre 4206, 4208 e 4212 sono simili ai dolori toracici. Di seguito viene illustrato come interpretare gli eventi 4202 e 4204 sotto.

### <a name="staging-area-events"></a>Eventi dell'area di gestione temporanea

> ID evento: **4202**  
> Gravità: **avviso**
> 
> Il servizio Replica DFS ha rilevato che lo spazio di gestione temporanea usato per la cartella replicata nel percorso locale (percorso) è superiore al limite massimo. Il servizio tenterà di eliminare i file di staging meno recenti. Potrebbero essere interessate le prestazioni.
> 
> ID evento: **4204**  
> Gravità: **informativo**
> 
> Il servizio Replica DFS ha eliminato correttamente i vecchi file di gestione temporanea per la cartella replicata nel percorso locale (percorso). Lo spazio di gestione temporanea è ora al di sotto del limite massimo.
> 
> ID evento: **4206**  
> Gravità: **avviso**
> 
> Il servizio Replica DFS non è riuscito a pulire i file di staging obsoleti per la cartella replicata nel percorso locale (percorso). Il servizio potrebbe non riuscire a replicare alcuni file di grandi dimensioni e la cartella replicata potrebbe non essere sincronizzata. Il servizio ritenterà automaticamente la pulizia dello spazio di staging in (x) minuti. Il servizio può avviare la pulizia in un secondo tempo se rileva che alcuni file di staging sono stati sbloccati.
> 
> ID evento: **4208**  
> Gravità: **avviso**
> 
> Il servizio Replica DFS ha rilevato che l'utilizzo dello spazio di gestione temporanea è superiore alla quota di gestione temporanea per la cartella replicata nel percorso locale (percorso). Il servizio potrebbe non riuscire a replicare alcuni file di grandi dimensioni e la cartella replicata potrebbe non essere sincronizzata. Il servizio tenterà di pulire automaticamente lo spazio di gestione temporanea.
> 
> ID evento: **4212**  
> Gravità: **errore**
> 
> Il servizio Replica DFS non è stato in grado di replicare la cartella replicata nel percorso locale (percorso) perché il percorso di gestione temporanea non è valido o è inaccessibile.

## <a name="what-is-the-difference-between-4202-and-4208"></a>Qual è la differenza tra 4202 e 4208?

Gli eventi 4202 e 4208 hanno testo simile; ad esempio, DFSR ha rilevato che l'utilizzo dell'area di gestione temporanea supera il limite massimo. La differenza è che 4208 viene registrato dopo l'esecuzione della pulizia dell'area di gestione temporanea e la quota di gestione temporanea viene ancora superata. 4202 è un evento normale e previsto, mentre 4208 è anomalo e richiede un intervento.

## <a name="how-many-4202-4204-events-are-too-many"></a>Quanti eventi 4202, 4204 sono troppi?

Non esiste una singola risposta a questa domanda. A differenza degli eventi 4206, 4208 o 4212, che sono sempre errati e indicano che è necessaria un'azione, gli eventi 4202 e 4204 si verificano in condizioni operative normali. La visualizzazione di molti eventi 4202 e 4204 *può* indicare un problema. Ecco alcuni aspetti da considerare:

1.  La registrazione della cartella replicata (RF) 4202 esegue la replica iniziale? In tal caso, è normale registrare gli eventi 4202 e 4204. È consigliabile evitare il minor numero possibile di operazioni durante la replica iniziale, fornendo la maggior parte dell'area di gestione temporanea possibile
2.  Il semplice controllo del numero totale di eventi 4202 non è sufficiente. È necessario verificare quanti sono stati registrati per RF. Se si registrano 20 4202 eventi per uno RF in un periodo di 24 ore che è elevato. Tuttavia, se si dispone di 20 cartelle replicate ed è presente un evento per ogni cartella, si sta eseguendo correttamente questa operazione.
3.  È necessario esaminare diversi giorni di dati per stabilire le tendenze.

Generalmente Consiglio ai clienti di non consentire più di 1 4202 eventi per ogni cartella replicata al giorno in condizioni operative normali. "Normale" significa che non è in corso alcuna replica iniziale. Si basa su questo ragionamento:

1.  Il tempo impiegato per pulire l'area di gestione temporanea è il tempo impiegato per la replica dei file. La replica viene sospesa mentre l'area di gestione temporanea viene cancellata.
2.  DFSR trae vantaggio da un'area di gestione temporanea completa che la utilizza per la RDC e la RDC tra file o la replica degli stessi file in altri membri
3.  Maggiore è il numero di eventi 4202 e 4204 è possibile registrare maggiori sono le probabilità che si verifichino nella condizione in cui DFSR non è in grado di eliminare l'area di gestione temporanea o deve eliminare i file dall'area di gestione temporanea in modo anomalo.
4.  gli eventi 4206, 4208 e 4212 sono, in esperienza, sempre preceduti e seguiti da un numero elevato di eventi 4202 e 4204.

Sebbene sia consentita solo l'evento 1 4202 per RF al giorno, si riduce in modo sostanziale le probabilità che si verifichino problemi nell'area di gestione temporanea e si utilizzi meglio le risorse del server DFSR per lo scopo previsto della replica dei file.


