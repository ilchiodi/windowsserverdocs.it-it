---
title: Comandi di Windows
description: Riferimento
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/09/2020
ms.prod: windows-server
ms.openlocfilehash: 66de80652f764840af70e88dfd39df2398ae495c
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721115"
---
# <a name="windows-commands"></a>Comandi di Windows

Tutte le versioni supportate di Windows (server e client) dispongono di un set di comandi console Win32 incorporati.

In questo set di documentazione vengono descritti i comandi di Windows che è possibile utilizzare per automatizzare le attività tramite script o strumenti di scripting.

Per trovare informazioni su un comando specifico, nel menu A-Z seguente, fare clic sulla lettera che il comando inizia con e quindi fare clic sul nome di comando.

[Oggetto](#a)  |
 [B](#b)  |
 [C](#c)  |
 [D](#d)  |
 [E](#e)  |
 [F](#f)  |
 [G](#g)  |
 [H](#h)  |
 [I](#i)  |
 [J](#j)  |
 [K](#k)  |
 [L](#l)  |
 [M](#m)  |
 [N](#n)  |
 [O](#o)  |
 [P](#p)  |
 [D](#q)  |
 [R](#r)  |
 [S](#s)  |
 [T](#t)  |
 [U](#u)  |
 [V](#v)  |
 [W](#w)  |
 [X](#x) | Y | Z

## <a name="prerequisites"></a>Prerequisiti

Le informazioni contenute in questo argomento si applicano a:

- Windows Server 2019
- Windows Server (Canale semestrale)
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows 10
- Windows 8.1

### <a name="command-shell-overview"></a>Cenni preliminari sulla shell comandi

La shell dei comandi era la prima Shell incorporata in Windows per automatizzare le attività di routine, ad esempio la gestione degli account utente o i backup notturni, con file batch (. bat). Con Windows script host è possibile eseguire script più sofisticati nella shell dei comandi. Per ulteriori informazioni, vedere [cscript](cscript.md) o [WScript](wscript.md). È possibile eseguire operazioni in modo più efficiente utilizzando gli script rispetto a quanto possibile tramite l'interfaccia utente. Gli script accettano tutti i comandi disponibili nella riga di comando.

Windows include due shell dei comandi: la shell dei comandi e [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Ogni shell è un programma software che fornisce la comunicazione diretta tra l'utente e il sistema operativo o l'applicazione, fornendo un ambiente per automatizzare le operazioni IT.

PowerShell è stato progettato per estendere le funzionalità della shell dei comandi per eseguire comandi di PowerShell denominati cmdlet. I cmdlet sono simili ai comandi di Windows, ma forniscono un linguaggio di scripting più estendibile. È possibile eseguire i comandi di Windows e i cmdlet di PowerShell in PowerShell, ma la shell dei comandi può eseguire solo i comandi di Windows e non i cmdlet di PowerShell.

Per l'automazione di Windows più affidabile e aggiornata, è consigliabile usare PowerShell anziché i comandi di Windows o Windows script host per l'automazione di Windows.

> [!NOTE]
>È anche possibile scaricare e installare [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), la versione open source di PowerShell.

> [!CAUTION]
> La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare le modifiche seguenti al registro di sistema, è necessario eseguire il backup di tutti i dati importanti presenti nel computer.

> [!NOTE]
> Per abilitare o disabilitare il completamento del nome file e directory nella shell dei comandi in una sessione di accesso utente o computer, eseguire **regedit.exe** e impostare il **valore reg_DWOrd**seguente:
>
> HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\completionChar\ reg_DWOrd
>
> Per impostare il valore **reg_DWOrd** , utilizzare il valore esadecimale di un carattere di controllo per una funzione specifica (ad esempio, **0 9** è Tab e **0 08** è Backspace). Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

## <a name="command-line-reference-a-z"></a>Riferimento della riga di comando A-Z

Per trovare informazioni su un comando di Windows specifico, nel menu A-Z seguente fare clic sulla lettera che il comando inizia con e quindi fare clic sul nome del comando.

[Oggetto](#a)  |
 [B](#b)  |
 [C](#c)  |
 [D](#d)  |
 [E](#e)  |
 [F](#f)  |
 [G](#g)  |
 [H](#h)  |
 [I](#i)  |
 [J](#j)  |
 [K](#k)  |
 [L](#l)  |
 [M](#m)  |
 [N](#n)  |
 [O](#o)  |
 [P](#p)  |
 [D](#q)  |
 [R](#r)  |
 [S](#s)  |
 [T](#t)  |
 [U](#u)  |
 [V](#v)  |
 [W](#w)  |
 [X](#x) | Y | Z

### <a name="a"></a>A

- [active](active.md)
- [add](add.md)
- [add alias](add-alias.md)
- [add volume](add-volume.md)
- [append](append.md)
- [arp](arp.md)
- [assign](assign.md)
- [assoc](assoc.md)
- [at](at.md)
- [atmadm](atmadm.md)
- [attach-vdisk](attach-vdisk.md)
- [attrib](attrib.md)
- [attributes](attributes.md)
  - [attributes disk](attributes-disk.md)
  - [attributes volume](attributes-volume.md)
- [auditpol](auditpol.md)
  - [auditpol backup](auditpol-backup.md)
  - [auditpol clear](auditpol-clear.md)
  - [auditpol get](auditpol-get.md)
  - [auditpol list](auditpol-list.md)
  - [auditpol remove](auditpol-remove.md)
  - [auditpol resourcesacl](auditpol-resourcesacl.md)
  - [auditpol restore](auditpol-restore.md)
  - [auditpol set](auditpol-set.md)
- [autochk](autochk.md)
- [autoconv](autoconv.md)
- [autofmt](autofmt.md)
- [automount](automount.md)

### <a name="b"></a>B

- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
  - [bdehdcfg driveinfo](bdehdcfg-driveinfo.md)
  - [bdehdcfg newdriveletter](bdehdcfg-newdriveletter.md)
  - [bdehdcfg quiet](bdehdcfg-quiet.md)
  - [bdehdcfg restart](bdehdcfg-restart.md)
  - [bdehdcfg size](bdehdcfg-size.md)
  - [bdehdcfg target](bdehdcfg-target.md)
- [begin backup](begin-backup.md)
- [begin restore](begin-restore.md)
- [bitsadmin](bitsadmin.md)
  - [bitsadmin addfile](bitsadmin-addfile.md)
  - [bitsadmin addfileset](bitsadmin-addfileset.md)
  - [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  - [bitsadmin cache](bitsadmin-cache.md)
    - [bitsadmin cache e delete](bitsadmin-cache-and-delete.md)
    - [bitsadmin cache e deleteurl](bitsadmin-cache-and-deleteurl.md)
    - [bitsadmin cache e getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
    - [bitsadmin cache e getlimit](bitsadmin-cache-and-getlimit.md)
    - [bitsadmin cache e help](bitsadmin-cache-and-help.md)
    - [bitsadmin cache e info](bitsadmin-cache-and-info.md)
    - [bitsadmin cache e list](bitsadmin-cache-and-list.md)
    - [bitsadmin cache e setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
    - [bitsadmin cache e setlimit](bitsadmin-cache-and-setlimit.md)
    - [bitsadmin cache e clear](bitsadmin-cache-clear.md)
  - [bitsadmin cancel](bitsadmin-cancel.md)
  - [bitsadmin complete](bitsadmin-complete.md)
  - [bitsadmin create](bitsadmin-create.md)
  - [Esempi di bitsadmin](bitsadmin-examples.md)
  - [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  - [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  - [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  - [bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)
  - [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  - [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  - [bitsadmin getcustomheaders](bitsadmin-getcustomheaders.md)
  - [bitsadmin getdescription](bitsadmin-getdescription.md)
  - [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  - [bitsadmin geterror](bitsadmin-geterror.md)
  - [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  - [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  - [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  - [bitsadmin gethelpertokenflags](bitsadmin-gethelpertokenflags.md)
  - [bitsadmin gethelpertokensid](bitsadmin-gethelpertokensid.md)
  - [bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
  - [bitsadmin getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)
  - [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  - [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  - [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  - [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  - [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  - [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  - [bitsadmin getowner](bitsadmin-getowner.md)
  - [bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)
  - [bitsadmin getpriority](bitsadmin-getpriority.md)
  - [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  - [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  - [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  - [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  - [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  - [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  - [bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)
  - [bitsadmin getstate](bitsadmin-getstate.md)
  - [bitsadmin gettemporaryname](bitsadmin-gettemporaryname.md)
  - [bitsadmin gettype](bitsadmin-gettype.md)
  - [bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)
  - [bitsadmin help](bitsadmin-help.md)
  - [bitsadmin info](bitsadmin-info.md)
  - [bitsadmin list](bitsadmin-list.md)
  - [bitsadmin listfiles](bitsadmin-listfiles.md)
  - [bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
  - [bitsadmin monitor](bitsadmin-monitor.md)
  - [bitsadmin nowrap](bitsadmin-nowrap.md)
  - [bitsadmin peercaching](bitsadmin-peercaching.md)
    - [bitsadmin peercaching e getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
    - [bitsadmin peercaching e help](bitsadmin-peercaching-and-help.md)
    - [bitsadmin peercaching e setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
  - [bitsadmin peers](bitsadmin-peers.md)
    - [bitsadmin peers e clear](bitsadmin-peers-and-clear.md)
    - [bitsadmin peers e discover](bitsadmin-peers-and-discover.md)
    - [bitsadmin peers e help](bitsadmin-peers-and-help.md)
    - [bitsadmin peers e list](bitsadmin-peers-and-list.md)
  - [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  - [bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)
  - [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  - [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  - [bitsadmin reset](bitsadmin-reset.md)
  - [bitsadmin resume](bitsadmin-resume.md)
  - [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  - [bitsadmin setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)
  - [bitsadmin setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)
  - [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  - [bitsadmin setcustomheaders](bitsadmin-setcustomheaders.md)
  - [bitsadmin setdescription](bitsadmin-setdescription.md)
  - [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  - [bitsadmin sethelpertoken](bitsadmin-sethelpertoken.md)
  - [bitsadmin sethelpertokenflags](bitsadmin-sethelpertokenflags.md)
  - [bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
  - [bitsadmin setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)
  - [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  - [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  - [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  - [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  - [bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)
  - [bitsadmin setpriority](bitsadmin-setpriority.md)
  - [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  - [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  - [bitsadmin setsecurityflags](bitsadmin-setsecurityflags.md)
  - [bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)
  - [bitsadmin suspend](bitsadmin-suspend.md)
  - [bitsadmin takeownership](bitsadmin-takeownership.md)
  - [bitsadmin transfer](bitsadmin-transfer.md)
  - [bitsadmin util](bitsadmin-util.md)
    - [bitsadmin util e enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
    - [bitsadmin util e getieproxy](bitsadmin-util-and-getieproxy.md)
    - [bitsadmin util e help](bitsadmin-util-and-help.md)
    - [bitsadmin util e repairservice](bitsadmin-util-and-repairservice.md)
    - [bitsadmin util e setieproxy](bitsadmin-util-and-setieproxy.md)
    - [bitsadmin util e version](bitsadmin-util-and-version.md)
  - [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  - [bootcfg addsw](bootcfg-addsw.md)
  - [bootcfg copy](bootcfg-copy.md)
  - [bootcfg dbg1394](bootcfg-dbg1394.md)
  - [bootcfg debug](bootcfg-debug.md)
  - [bootcfg default](bootcfg-default.md)
  - [bootcfg delete](bootcfg-delete.md)
  - [bootcfg ems](bootcfg-ems.md)
  - [bootcfg query](bootcfg-query.md)
  - [bootcfg raw](bootcfg-raw.md)
  - [bootcfg rmsw](bootcfg-rmsw.md)
  - [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C

- [cacls](cacls.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  - [change logon](change-logon.md)
  - [change port](change-port.md)
  - [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [clean](clean.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [compatta vdisk](compact-vdisk.md)
- [convert](convert.md)
  - [convert basic](convert-basic.md)
  - [Converti dinamica](convert-dynamic.md)
  - [convert gpt](convert-gpt.md)
  - [convert mbr](convert-mbr.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [create](create.md)
  - [creare una partizione EFI](create-partition-efi.md)
  - [creare una [partizione estesa](create-partition-extended.md)
  - [Crea partizione logica](create-partition-logical.md)
  - [Crea partizione MSR](create-partition-msr.md)
  - [create partition primary](create-partition-primary.md)
  - [Crea mirror del volume](create-volume-mirror.md)
  - [Crea RAID del volume](create-volume-raid.md)
  - [Crea volume semplice](create-volume-simple.md)
  - [Crea Stripe del volume](create-volume-stripe.md)
- [cscript](cscript.md)

### <a name="d"></a>D

- [date](date.md)
- [dcgpofix](dcgpofix.md)
- [defrag](defrag.md)
- [del](del.md)
- [delete](delete.md)
  - [Elimina disco](delete-disk.md)
  - [Elimina partizione](delete-partition.md)
  - [Elimina ombre](delete-shadows.md)
  - [delete volume](delete-volume.md)
- [Scollega vdisk](detach-vdisk.md)
- [dettaglio](detail.md)
  - [disco dettagli](detail-disk.md)
  - [partizione dettagli](detail-partition.md)
  - [Dettagli vdisk](detail-vdisk.md)
  - [volume dettaglio](detail-volume.md)
- [Dfsdiag](dfsdiag.md)
  - [testdcs Dfsdiag](dfsdiag-testdcs.md)
  - [testdfsconfig Dfsdiag](dfsdiag-testdfsconfig.md)
  - [testdfsintegrity Dfsdiag](dfsdiag-testdfsintegrity.md)
  - [testreferral Dfsdiag](dfsdiag-testreferral.md)
  - [testsites Dfsdiag](dfsdiag-testsites.md)
- [dfsrmig](dfsrmig.md)
- [diantz](diantz.md)
- [dir](dir.md)
- [diskcomp](diskcomp.md)
- [diskcopy](diskcopy.md)
- [diskpart](diskpart.md)
- [diskperf](diskperf.md)
- [diskraid](diskraid.md)
- [diskshadow](diskshadow.md)
- [dispdiag](dispdiag.md)
- [dnscmd](dnscmd.md)
- [doskey](doskey.md)
- [driverquery](driverquery.md)

### <a name="e"></a>E

- [echo](echo.md)
- [edit](edit.md)
- [endlocal](endlocal.md)
- [fine ripristino](end-restore.md)
- [erase](erase.md)
- [eventcreate](eventcreate.md)
- [eventquery](eventquery.md)
- [eventtriggers](eventtriggers.md)
- [Evntcmd](evntcmd.md)
- [exec](exec.md)
- [exit](exit_2.md)
- [expand](expand.md)
- [Espandi vdisk](expand-vdisk.md)
- [esporre](expose.md)
- [extend](extend.md)
- [extract](extract.md)

### <a name="f"></a>F

- [fc](fc.md)
- [filesystems](filesystems.md)
- [find](find.md)
- [findstr](findstr.md)
- [finger](finger.md)
- [flattemp](flattemp.md)
- [fondue](fondue.md)
- [for](for.md)
- [forfiles](forfiles.md)
- [format](format.md)
- [freedisk](freedisk.md)
- [fsutil](fsutil.md)
  - [fsutil 8dot3name](fsutil-8dot3name.md)
  - [fsutil behavior](fsutil-behavior.md)
  - [fsutil dirty](fsutil-dirty.md)
  - [fsutil file](fsutil-file.md)
  - [fsutil fsinfo](fsutil-fsinfo.md)
  - [fsutil hardlink](fsutil-hardlink.md)
  - [fsutil objectid](fsutil-objectid.md)
  - [fsutil quota](fsutil-quota.md)
  - [fsutil repair](fsutil-repair.md)
  - [fsutil reparsepoint](fsutil-reparsepoint.md)
  - [fsutil resource](fsutil-resource.md)
  - [fsutil sparse](fsutil-sparse.md)
  - [fsutil tiering](fsutil-tiering.md)
  - [fsutil transaction](fsutil-transaction.md)
  - [fsutil usn](fsutil-usn.md)
  - [fsutil volume](fsutil-volume.md)
  - [fsutil wim](fsutil-wim.md)
- [ftp](ftp.md)
  - [ftp append](ftp-append.md)
  - [ftp ascii](ftp-ascii.md)
  - [ftp bell](ftp-bell_1.md)
  - [ftp binary](ftp-binary.md)
  - [ftp bye](ftp-bye.md)
  - [ftp cd](ftp-cd.md)
  - [ftp close](ftp-close_1.md)
  - [ftp debug](ftp-debug.md)
  - [ftp delete](ftp-delete.md)
  - [ftp dir](ftp-dir.md)
  - [ftp disconnect](ftp-disconnect_1.md)
  - [ftp get](ftp-get.md)
  - [ftp glob](ftp-glob_1.md)
  - [ftp hash](ftp-hash_1.md)
  - [ftp lcd](ftp-lcd.md)
  - [ftp literal](ftp-literal_1.md)
  - [ftp ls](ftp-ls_1.md)
  - [ftp mget](ftp-mget.md)
  - [ftp mkdir](ftp-mkdir.md)
  - [ftp mls](ftp-mls_1.md)
  - [ftp mput](ftp-mput_1.md)
  - [ftp open](ftp-open_1.md)
  - [ftp prompt](ftp-prompt_1.md)
  - [ftp put](ftp-put.md)
  - [ftp pwd](ftp-pwd_1.md)
  - [ftp quit](ftp-quit.md)
  - [ftp quote](ftp-quote.md)
  - [ftp recv](ftp-recv.md)
  - [ftp remotehelp](ftp-remotehelp_1.md)
  - [ftp rename](ftp-rename.md)
  - [ftp rmdir](ftp-rmdir.md)
  - [ftp send](ftp-send_1.md)
  - [ftp status](ftp-status.md)
  - [ftp trace](ftp-trace_1.md)
  - [ftp type](ftp-type.md)
  - [ftp user](ftp-user.md)
  - [ftp verbose](ftp-verbose_1.md)
  - [ftp mdelete](ftp-.mdelete_1.md)
  - [ftp mdir](ftp.mdir.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>G

- [getmac](getmac.md)
- [gettype](gettype.md)
- [goto](goto.md)
- [gpfixup](gpfixup.md)
- [gpresult](gpresult.md)
- [gpt](gpt.md)
- [gpupdate](gpupdate.md)
- [graftabl](graftabl.md)

### <a name="h"></a>H

- [help](help.md)
- [helpctr](helpctr.md)
- [hostname](hostname.md)

### <a name="i"></a>I

- [icacls](icacls.md)
- [if](if.md)
- [importazione (Shadowdisk)](import.md)
- [importazione (Diskpart)](import_1.md)
- [inactive](inactive.md)
- [inuse](inuse.md)
- [ipconfig](ipconfig.md)
- [ipxroute](ipxroute.md)
- [irftp](irftp.md)

### <a name="j"></a>J

- [jetpack](jetpack.md)

### <a name="k"></a>K

- [klist](klist.md)
- [ksetup](ksetup.md)
  - [ksetup addenctypeattr](ksetup-addenctypeattr.md)
  - [ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md)
  - [ksetup addkdc](ksetup-addkdc.md)
  - [ksetup addkpasswd](ksetup-addkpasswd.md)
  - [ksetup addrealmflags](ksetup-addrealmflags.md)
  - [ksetup changepassword](ksetup-changepassword.md)
  - [ksetup delenctypeattr](ksetup-delenctypeattr.md)
  - [ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md)
  - [ksetup delkdc](ksetup-delkdc.md)
  - [ksetup delkpasswd](ksetup-delkpasswd.md)
  - [ksetup delrealmflags](ksetup-delrealmflags.md)
  - [ksetup domain](ksetup-domain.md)
  - [ksetup dumpstate](ksetup-dumpstate.md)
  - [ksetup getenctypeattr](ksetup-getenctypeattr.md)
  - [ksetup listrealmflags](ksetup-listrealmflags.md)
  - [ksetup mapuser](ksetup-mapuser.md)
  - [ksetup removerealm](ksetup-removerealm.md)
  - [ksetup server](ksetup-server.md)
  - [ksetup setcomputerpassword](ksetup-setcomputerpassword.md)
  - [ksetup setenctypeattr](ksetup-setenctypeattr.md)
  - [ksetup setrealm](ksetup-setrealm.md)
  - [ksetup setrealmflags](ksetup-setrealmflags.md)
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L

- [label](label.md)
- [list](list.md)
  - [elenca i provider](list-providers.md)
  - [ombreggiatura elenco](list-shadows.md)
  - [elencare i writer](list-writers.md)
- [carica metadati](load-metadata.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  - [logman create](logman-create.md)
  - [Logman crea avviso](logman-create-alert.md)
  - [Logman creare api](logman-create-api.md)
  - [Logman creare cfg](logman-create-cfg.md)
  - [Logman creazione del contatore](logman-create-counter.md)
  - [Logman creare traccia](logman-create-trace.md)
  - [logman delete](logman-delete.md)
  - [importazione Logman e Logman Export](logman-import-export.md)
  - [logman query](logman-query.md)
  - [logman start e logman stop](logman-start-stop.md)
  - [logman update](logman-update.md)
  - [avviso di aggiornamento Logman](logman-update-alert.md)
  - [Logman aggiornare api](logman-update-api.md)
  - [Logman update cfg](logman-update-cfg.md)
  - [Logman update contatore](logman-update-counter.md)
  - [Logman update trace](logman-update-trace.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M

- [macfile](macfile.md)
- [makecab](makecab.md)
- [Gestisci bde](manage-bde.md)
  - [Gestisci stato BDE](manage-bde-status.md)
  - [Gestisci bde in](manage-bde-on.md)
  - [Gestisci bde disattivato](manage-bde-off.md)
  - [gestione della sospensione BDE](manage-bde-pause.md)
  - [Gestisci bde resume](manage-bde-resume.md)
  - [Gestisci blocco BDE](manage-bde-lock.md)
  - [gestione sblocco BDE](manage-bde-unlock.md)
  - [Gestisci sblocco automatico BDE](manage-bde-autounlock.md)
  - [Gestisci protezioni BDE](manage-bde-protectors.md)
  - [Gestisci TPM BDE](manage-bde-tpm.md)
  - [Gestisci ID di stato BDE](manage-bde-setidentifier.md)
  - [gestire forcerecovery BDE](manage-bde-forcerecovery.md)
  - [Gestisci bde di BDE](manage-bde-changepassword.md)
  - [gestire changepin Aggiorna BDE](manage-bde-changepin.md)
  - [gestire ChangeKey BDE](manage-bde-changekey.md)
  - [Gestisci pacchetto di pacchetti BDE](manage-bde-keypackage.md)
  - [Gestisci aggiornamento BDE](manage-bde-upgrade.md)
  - [gestire wipefreespace BDE](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [MD](md.md)
- [Unisci vdisk](merge-vdisk.md)
- [mkdir](mkdir.md)
- [mklink](mklink.md)
- [mmc](mmc.md)
- [mode](mode.md)
- [more](more.md)
- [mount](mount.md)
- [mountvol](mountvol.md)
- [move](move.md)
- [mqbkup](mqbkup.md)
- [mqsvc](mqsvc.md)
- [mqtgsvc](mqtgsvc.md)
- [msdt](msdt.md)
- [msg](msg.md)
- [msiexec](msiexec.md)
- [msinfo32](msinfo32.md)
- [mstsc](mstsc.md)

### <a name="n"></a>N

- [nbtstat](nbtstat.md)
- [netcfg](netcfg.md)
- [stampa NET](net-print.md)
- [netsh](netsh.md)
- [netstat](netstat.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  - [Comando nslookup exit](nslookup-exit-command.md)
  - [Comando nslookup finger](nslookup-finger-command.md)
  - [nslookup help](nslookup-help.md)
  - [nslookup ls](nslookup-ls.md)
  - [nslookup lserver](nslookup-lserver.md)
  - [nslookup root](nslookup-root.md)
  - [nslookup server](nslookup-server.md)
  - [nslookup set](nslookup-set.md)
  - [nslookup set all](nslookup-set-all.md)
  - [nslookup set class](nslookup-set-class.md)
  - [nslookup set d2](nslookup-set-d2.md)
  - [nslookup set debug](nslookup-set-debug.md)
  - [nslookup set domain](nslookup-set-domain.md)
  - [nslookup set port](nslookup-set-port.md)
  - [nslookup set querytype](nslookup-set-querytype.md)
  - [nslookup set recurse](nslookup-set-recurse.md)
  - [nslookup set retry](nslookup-set-retry.md)
  - [nslookup set root](nslookup-set-root.md)
  - [nslookup set search](nslookup-set-search.md)
  - [nslookup set srchlist](nslookup-set-srchlist.md)
  - [nslookup set timeout](nslookup-set-timeout.md)
  - [nslookup set type](nslookup-set-type.md)
  - [nslookup set vc](nslookup-set-vc.md)
  - [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O

- [offline](offline.md)
  - [disco offline](offline-disk.md)
  - [volume offline](offline-volume.md)
- [online](online.md)
  - [disco online](online-disk.md)
  - [volume online](online-volume.md)
- [openfiles](openfiles.md)

### <a name="p"></a>P

- [pagefileconfig](pagefileconfig.md)
- [path](path.md)
- [pathping](pathping.md)
- [pause](pause.md)
- [pbadmin](pbadmin.md)
- [pentnt](pentnt.md)
- [perfmon](perfmon.md)
- [ping](ping.md)
- [pnpunattend](pnpunattend.md)
- [pnputil](pnputil.md)
- [popd](popd.md)
- [PowerShell](powershell.md)
- [PowerShell ISE](powershell_ise.md)
- [print](print.md)
- [prncnfg](prncnfg.md)
- [prndrvr](prndrvr.md)
- [prnjobs](prnjobs.md)
- [prnmngr](prnmngr.md)
- [prnport](prnport.md)
- [prnqctl](prnqctl.md)
- [prompt](prompt.md)
- [pubprn](pubprn.md)
- [pushd](pushd.md)
- [pushprinterconnections](pushprinterconnections.md)
- [pwlauncher](pwlauncher.md)

### <a name="q"></a>Q

- [qappsrv](qappsrv.md)
- [qprocess](qprocess.md)
- [query](query.md)
  - [processo query](query-process.md)
  - [sessione di query](query-session.md)
  - [termserver query](query-termserver.md)
  - [utente query](query-user.md)
- [quser](quser.md)
- [qwinsta](qwinsta.md)

### <a name="r"></a>R

- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [ripristino del gruppo di dischi](recover_1.md)
- [reg](reg.md)
  - [reg Aggiungi](reg-add.md)
  - [confronto reg](reg-compare.md)
  - [Copia reg](reg-copy.md)
  - [reg delete](reg-delete.md)
  - [esportazione reg](reg-export.md)
  - [importazione reg](reg-import.md)
  - [carico reg](reg-load.md)
  - [query reg](reg-query.md)
  - [ripristino reg](reg-restore.md)
  - [reg Salva](reg-save.md)
  - [Scaricamento reg](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [file batch REM](rem.md)
- [script REM](rem_1.md)
- [remove](remove.md)
- [ren](ren.md)
- [rename](rename.md)
- [ripristino](repair.md)
  - [ripristinare BDE](repair-bde.md)
- [replace](replace.md)
- [Ripeti analisi](rescan.md)
- [reset](reset.md)
  - [reset session](reset-session.md)
- [mantenere](retain.md)
- [ripristinare](revert.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [WS2008 Route](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [printui rundll32](rundll32-printui.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S

- [San](san.md)
- [configurazione SC](sc-config.md)
- [creazione SC](sc-create.md)
- [eliminazione SC](sc-delete.md)
- [Query SC](sc-query.md)
- [schtasks](schtasks.md)
- [scwcmd](scwcmd.md)
  - [scwcmd analyze](scwcmd-analyze.md)
  - [configurazione di Scwcmd](scwcmd-configure.md)
  - [registro scwcmd](scwcmd-register.md)
  - [rollback scwcmd](scwcmd-rollback.md)
  - [trasformazione scwcmd](scwcmd-transform.md)
  - [visualizzazione scwcmd](scwcmd-view.md)
- [secedit](secedit.md)
  - [analizza Secedit](secedit-analyze.md)
  - [configurazione Secedit](secedit-configure.md)
  - [esportazione Secedit](secedit-export.md)
  - [secedit GenerateRollback](secedit-generaterollback.md)
  - [importazione Secedit](secedit-import.md)
  - [convalida Secedit](secedit-validate.md)
- [select](select.md)
  - [select disk](select-disk.md)
  - [Seleziona partizione](select-partition.md)
  - [Seleziona vdisk](select-vdisk.md)
  - [select volume](select-volume.md)
- [serverceipoptin](serverceipoptin.md)
- [ServerManagerCmd](servermanagercmd.md)
- [serverWerOptin](serverweroptin.md)
- [impostare le variabili di ambiente](set_1.md)
- [imposta copia shadow](set_2.md)
  - [Imposta contesto](set-context.md)
  - [imposta ID](set-id.md)
  - [setlocal](setlocal.md)
  - [Imposta metadati](set-metadata.md)
  - [opzione set](set-option.md)
  - [imposta verbose](set-verbose.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shrink](shrink.md)
- [shutdown](shutdown.md)
- [simula ripristino](simulate-restore.md)
- [sort](sort.md)
- [start](start.md)
- [set di sottocomandi dispositivo](subcommand-set-device.md)
- [set di sottocomandi drivergroup](subcommand-set-drivergroup.md)
- [set di sottocomandi drivergroupfilter](subcommand-set-drivergroupfilter.md)
- [set di sottocomandi (DriverPack)](subcommand-set-driverpackage.md)
- [immagine del set di sottocomandi](subcommand-set-image.md)
- [set di sottocomandi ImageGroup](subcommand-set-imagegroup.md)
- [server di set di sottocomandi](subcommand-set-server.md)
- [set di sottocomandi TransportServer](subcommand-set-transportserver.md)
- [set di sottocomandi MulticastTransmission](subcommand-start-multicasttransmission.md)
- [spazio dei nomi avvio sottocomando](subcommand-start-namespace.md)
- [server di avvio sottocomando](subcommand-start-server.md)
- [sottocomando Start TransportServer](subcommand-start-transportserver.md)
- [server di arresto sottocomando](subcommand-stop-server.md)
- [sottocomando stop TransportServer](subcommand-stop-transportserver.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)


### <a name="t"></a>T

- [takeown](takeown.md)
- [tapicfg](tapicfg.md)
- [taskkill](taskkill.md)
- [tasklist](tasklist.md)
- [tcmsetup](tcmsetup.md)
- [telnet](telnet.md)
  - [chiusura Telnet](telnet-close.md)
  - [visualizzazione Telnet](telnet-display.md)
  - [Telnet aperto](telnet-open.md)
  - [uscita Telnet](telnet-quit.md)
  - [invio Telnet](telnet-send.md)
  - [set Telnet](telnet-set.md)
  - [stato Telnet](telnet-status.md)
  - [Telnet non impostato](telnet-unset.md)
- [tftp](tftp.md)
- [time](time.md)
- [timeout](timeout_1.md)
- [title](title_1.md)
- [tlntadmn](tlntadmn.md)
- [tpmtool](tpmtool.md)
- [tpmvscmgr](tpmvscmgr.md)
- [tracerpt](tracerpt_1.md)
- [tracert](tracert.md)
- [tree](tree.md)
- [tscon](tscon.md)
- [tsdiscon](tsdiscon.md)
- [tsecimp](tsecimp_1.md)
- [tskill](tskill.md)
- [tsprof](tsprof.md)
- [type](type.md)
- [typeperf](typeperf.md)
- [tzutil](tzutil.md)

### <a name="u"></a>U

- [volume x:.](unexpose.md)
- [UniqueId](uniqueid.md)
- [unlodctr](unlodctr_1.md)

### <a name="v"></a>V

- [ver](ver.md)
- [verifier](verifier.md)
- [verify](verify_1.md)
- [vol](vol.md)
- [vssadmin](vssadmin.md)
  - [vssadmin delete shadows](vssadmin-delete-shadows.md)
  - [vssadmin list shadows](vssadmin-list-shadows.md)
  - [vssadmin list writers](vssadmin-list-writers.md)
  - [vssadmin resize shadowstorage](vssadmin-resize-shadowstorage.md)

### <a name="w"></a>W

- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  - [wbadmin Elimina catalogo](wbadmin-delete-catalog.md)
  - [Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  - [wbadmin Disabilita backup](wbadmin-disable-backup.md)
  - [wbadmin Abilita backup](wbadmin-enable-backup.md)
  - [comando Wbadmin get Disks](wbadmin-get-disks.md)
  - [wbadmin Ottieni elementi](wbadmin-get-items.md)
  - [stato Wbadmin get](wbadmin-get-status.md)
  - [Wbadmin get versions](wbadmin-get-versions.md)
  - [Wbadmin restore catalog](wbadmin-restore-catalog.md)
  - [wbadmin avviare il backup](wbadmin-start-backup.md)
  - [wbadmin Avvia ripristino](wbadmin-start-recovery.md)
  - [comando Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)
  - [comando Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)
  - [comando Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  - [wbadmin Arresta processo](wbadmin-stop-job.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [WinSAT MEM](winsat-mem.md)
- [mfmedia WinSAT](winsat-mfmedia.md)
- [wmic](wmic.md)
- [writer](writer.md)
- [wscript](wscript.md)

### <a name="x"></a>X

- [xcopy](xcopy.md)
