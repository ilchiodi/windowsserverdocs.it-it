---
Title: 'SMB: Le porte di condivisione file e stampanti devono essere aperte'
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: b6ad1f1f8573fc380e999e5ec2091cea8ebb8aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820362"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB: Le porte di condivisione file e stampanti devono essere aperte


Aggiornamento: 2 febbraio 2011

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, Windows Server 2008 R2

*Questo argomento è dedicato alla risoluzione di un problema specifico identificato da un'analisi di Best Practices Analyzer. È consigliabile applicare le informazioni contenute in questo argomento solo ai computer in cui è stato eseguito Best di File Services Best Practices Analyzer e si verifica il problema discusso in questo argomento. Per altre informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a).


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Sistema operativo</strong></p></td>
<td><p>Windows Server</p></td>
</tr>
<tr class="even">
<td><p><strong>/ Funzionalità del prodotto</strong></p></td>
<td><p>Servizi file</p></td>
</tr>
<tr class="odd">
<td><p><strong>Severity</strong></p></td>
<td><p>Errore</p></td>
</tr>
<tr class="even">
<td><p><strong>Categoria</strong></p></td>
<td><p>Configurazione</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>Problema

> *Le porte del firewall sono necessari per la condivisione file e stampanti non aprire (porte 139 e 445).*

## <a name="impact"></a>Impatto

> *I computer non sarà in grado di accedere alle cartelle condivise e altri servizi di rete basata su Server Message Block SMB su questo server.*

## <a name="resolution"></a>Risoluzione

> *Abilitare la condivisione File e stampanti per la comunicazione attraverso il firewall del computer.*

Il requisito minimo per completare questa procedura è l'appartenenza al gruppo **Administrators** locale o a un gruppo equivalente.

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>Per aprire le porte del firewall per abilitare la condivisione file e stampanti

1.  Aprire il pannello di controllo, fare clic su **sistema e sicurezza**, quindi fare clic su **Windows Firewall**.

2.  Nel riquadro sinistro, fare clic su **impostazioni avanzate**, nell'albero della console, fare clic su **regole connessioni in entrata**.

3.  Sotto **regole connessioni in entrata**, individuare le regole **condivisione File e stampanti (NB-sessione-In)** e **condivisione File e stampanti (SMB-In)**.

4.  Per ogni regola, fare doppio clic su regola e quindi fare clic su **Abilita regola**.

## <a name="additional-references"></a>Altri riferimenti

[Informazioni sulle cartelle condivise e il Firewall Windows](http://technet.microsoft.com/en-us/library/cc731402.aspx)(http://technet.microsoft.com/en-us/library/cc731402.aspx)

