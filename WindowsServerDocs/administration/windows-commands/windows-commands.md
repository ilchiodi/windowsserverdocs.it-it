---
title: Comandi di Windows
description: Comandi di Windows
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/26/2019
ms.prod: windows-server
ms.openlocfilehash: 9d68e2becbf9c6522be7e1ff6e6742d44f3a8247
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829234"
---
# <a name="windows-commands"></a>Comandi di Windows

Tutte le versioni supportate di Windows (server e client) dispongono di un set di comandi console Win32 incorporati.

In questo set di documentazione vengono descritti i comandi di Windows che è possibile utilizzare per automatizzare le attività tramite script o strumenti di scripting.

Per trovare informazioni su un comando specifico, nel menu A-Z seguente, fare clic sulla lettera che il comando inizia con e quindi fare clic sul nome di comando.

[A](#a) |
[B](#b) | 
[C](#c) | 
[D](#d) | 
[E](#e) | 
[F](#f) | 
[G](#g) | 
[H](#h) | 
[i](#i) |
[J](#j) | 
[K](#k) | 
[L](#l) | 
[M](#m) | 
[N](#n) | 
[O](#o) | 
[P](#p) | 
[Q](#q) | 
[R](#r) | 
[S](#s) | 
[t](#t) | 
[U](#u) | 
[V](#v) | 
[W](#w) | 
[X](#x) | Y | Z

## <a name="prerequisites"></a>Prerequisiti

Le informazioni contenute in questo argomento si applicano a:

-   Windows Server 2019
-   Windows Server (Canale semestrale)
-   Windows Server 2016
-   Windows Server 2012 R2
-   Windows Server 2012 
-   Windows Server 2008 R2
-   Windows Server 2008
-   Windows 10
-   Windows 8.1

### <a name="command-shell-overview"></a>Cenni preliminari sulla shell comandi

La shell dei comandi era la prima Shell incorporata in Windows per automatizzare le attività di routine, ad esempio la gestione degli account utente o i backup notturni, con file batch (. bat). Con Windows script host è possibile eseguire script più sofisticati nella shell dei comandi. Per ulteriori informazioni, vedere [cscript](cscript.md) o [WScript](wscript.md). È possibile eseguire operazioni in modo più efficiente utilizzando gli script rispetto a quanto possibile tramite l'interfaccia utente. Gli script accettano tutti i comandi disponibili nella riga di comando.

Windows include due shell dei comandi: la shell dei comandi e [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Ogni shell è un programma software che fornisce la comunicazione diretta tra l'utente e il sistema operativo o l'applicazione, fornendo un ambiente per automatizzare le operazioni IT.

PowerShell è stato progettato per estendere le funzionalità della shell dei comandi per eseguire comandi di PowerShell denominati cmdlet. I cmdlet sono simili ai comandi di Windows, ma forniscono un linguaggio di scripting più estendibile. È possibile eseguire i comandi di Windows e i cmdlet di PowerShell in PowerShell, ma la shell dei comandi può eseguire solo i comandi di Windows e non i cmdlet di PowerShell.

Per l'automazione di Windows più affidabile e aggiornata, è consigliabile usare PowerShell anziché i comandi di Windows o Windows script host per l'automazione di Windows. 
> [!NOTE]
>È anche possibile scaricare e installare [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), la versione open source di PowerShell. 

> [!CAUTION]
> È possibile che eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare le modifiche seguenti al registro di sistema, è necessario eseguire il backup di tutti i dati importanti presenti nel computer.

> [!NOTE]
> Per abilitare o disabilitare il completamento del nome file e directory nella shell dei comandi in una sessione di accesso utente o computer, eseguire **Regedit. exe** e impostare il **valore di reg_DWOrd**seguente:
> 
> HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\completionChar\ reg_DWOrd
> 
> Per impostare il valore **reg_DWOrd** , utilizzare il valore esadecimale di un carattere di controllo per una funzione specifica (ad esempio, **0 9** è Tab e **0 08** è Backspace). Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

## <a name="command-line-reference-a-z"></a>Riferimento della riga di comando A-Z

Per trovare informazioni su un comando di Windows specifico, nel menu A-Z seguente fare clic sulla lettera che il comando inizia con e quindi fare clic sul nome del comando.

[A](#a) |
[B](#b) | 
[C](#c) | 
[D](#d) | 
[E](#e) | 
[F](#f) | 
[G](#g) | 
[H](#h) | 
[i](#i) |
[J](#j) | 
[K](#k) | 
[L](#l) | 
[M](#m) | 
[N](#n) | 
[O](#o) | 
[P](#p) | 
[Q](#q) | 
[R](#r) | 
[S](#s) | 
[t](#t) | 
[U](#u) | 
[V](#v) | 
[W](#w) | 
[X](#x) | Y | Z

### <a name="a"></a>A
-   [append](append.md)
-   [arp](arp.md)
-   [assoc](assoc.md)
-   [at](at.md)
-   [atmadm](atmadm.md)
-   [attrib](attrib.md)
-   [auditpol](auditpol.md)
-   [autochk](autochk.md)
-   [autoconv](autoconv.md)
-   [autofmt](autofmt.md)

### <a name="b"></a>B
- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
- [bitsadmin](bitsadmin.md)
  -   [bitsadmin addfile](bitsadmin-addfile.md)
  -   [bitsadmin addfileset](bitsadmin-addfileset.md)
  -   [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  -   [bitsadmin cancel](bitsadmin-cancel.md)
  -   [bitsadmin complete](bitsadmin-complete.md)
  -   [bitsadmin create](bitsadmin-create.md)
  -   [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  -   [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  -   [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  -   [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  -   [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  -   [bitsadmin getdescription](bitsadmin-getdescription.md)
  -   [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  -   [bitsadmin geterror](bitsadmin-geterror.md)
  -   [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  -   [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  -   [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  -   [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  -   [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  -   [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  -   [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  -   [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  -   [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  -   [bitsadmin getowner](bitsadmin-getowner.md)
  -   [Bitsadmin ottieni priorità](bitsadmin-getpriority.md)
  -   [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  -   [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  -   [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  -   [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  -   [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  -   [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  -   [bitsadmin getstate](bitsadmin-getstate.md)
  -   [bitsadmin gettype](bitsadmin-gettype.md)
  -   [bitsadmin help](bitsadmin-help.md)
  -   [bitsadmin info](bitsadmin-info.md)
  -   [bitsadmin list](bitsadmin-list.md)
  -   [bitsadmin listfiles](bitsadmin-listfiles.md)
  -   [bitsadmin monitor](bitsadmin-monitor.md)
  -   [bitsadmin nowrap](bitsadmin-nowrap.md)
  -   [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  -   [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  -   [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  -   [bitsadmin reset](bitsadmin-reset.md)
  -   [bitsadmin resume](bitsadmin-resume.md)
  -   [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  -   [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  -   [bitsadmin setdescription](bitsadmin-setdescription.md)
  -   [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  -   [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  -   [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  -   [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  -   [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  -   [bitsadmin setpriority](bitsadmin-setpriority.md)
  -   [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  -   [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  -   [bitsadmin suspend](bitsadmin-suspend.md)
  -   [bitsadmin takeownership](bitsadmin-takeownership.md)
  -   [Trasferimento Bitsadmin](bitsadmin-transfer.md)
  -   [bitsadmin util](bitsadmin-util.md)
  -   [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  -   [bootcfg addsw](bootcfg-addsw.md)
  -   [bootcfg copy](bootcfg-copy.md)
  -   [bootcfg dbg1394](bootcfg-dbg1394.md)
  -   [bootcfg debug](bootcfg-debug.md)  
  -   [bootcfg default](bootcfg-default.md)
  -   [bootcfg delete](bootcfg-delete.md)
  -   [bootcfg ems](bootcfg-ems.md)
  -   [bootcfg query](bootcfg-query.md)
  -   [bootcfg raw](bootcfg-raw.md)
  -   [bootcfg rmsw](bootcfg-rmsw.md)
  -   [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C
- [cacls](cacls_1.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  -   [change logon](change-logon.md)
  -   [change port](change-port.md)
  -   [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir_1.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [Cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [convert](convert.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [cscript](cscript.md)

### <a name="d"></a>D
-   [date](date.md)
-   [dcgpofix](dcgpofix.md)
-   [defrag](defrag.md)
-   [del](del.md)
-   [dfsrmig](dfsrmig.md)
-   [diantz](diantz.md)
-   [dir](dir.md)
-   [diskcomp](diskcomp.md)
-   [diskcopy](diskcopy.md)
-   [diskpart](diskpart.md)
-   [diskperf](diskperf.md)
-   [diskraid](diskraid.md)
-   [diskshadow](diskshadow.md)
-   [dispdiag](dispdiag.md)
-   [dnscmd](Dnscmd.md)
-   [doskey](doskey.md)
-   [driverquery](driverquery.md)

### <a name="e"></a>E
-   [echo](echo.md)
-   [edit](edit.md)
-   [endlocal](endlocal.md)
-   [erase](erase.md)
-   [eventcreate](eventcreate.md)
-   [eventquery](eventquery.md)
-   [eventtriggers](eventtriggers.md)
-   [Evntcmd](Evntcmd.md)
-   [exit](exit_2.md)
-   [expand](expand.md)
-   [extract](extract.md)

### <a name="f"></a>F
- [fc](fc.md)
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
  -   [fsutil 8dot3name](fsutil-8dot3name.md) 
  -   [fsutil behavior](fsutil-behavior.md) 
  -   [fsutil file](fsutil-file.md)
  -   [fsutil fsinfo](fsutil-fsinfo.md)
  -   [fsutil hardlink](fsutil-hardlink.md)
  -   [fsutil objectid](fsutil-objectid.md)
  -   [fsutil quota](fsutil-quota.md)
  -   [fsutil repair](fsutil-repair.md)
  -   [fsutil reparsepoint](fsutil-reparsepoint.md)
  -   [fsutil resource](fsutil-resource.md)
  -   [fsutil sparse](fsutil-sparse.md)
  -   [fsutil tiering](fsutil-tiering.md)
  -   [fsutil transaction](fsutil-transaction.md)
  -   [fsutil usn](fsutil-usn.md)
  -   [fsutil volume](fsutil-volume.md)
  -   [fsutil wim](fsutil-wim.md)
- [FTP](ftp.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>G
-   [getmac](getmac.md)
-   [gettype](gettype.md)
-   [goto](goto.md)
-   [gpfixup](gpfixup.md)
-   [gpresult](gpresult.md)
-   [gpupdate](gpupdate.md)
-   [graftabl](graftabl.md)

### <a name="h"></a>H
-   [help](help.md)
-   [helpctr](helpctr.md)
-   [hostname](hostname.md)

### <a name="i"></a>I
-   [icacls](icacls.md)
-   [if](if.md)
-   [inuse](inuse.md)
-   [ipconfig](ipconfig.md)
-   [ipxroute](ipxroute.md)
-   [irftp](irftp.md)

### <a name="j"></a>J
-   [jetpack](jetpack.md)

### <a name="k"></a>K
- [klist](klist.md)
- [ksetup](ksetup.md)
  -   [che Ksetup: Unreal](ksetup-setrealm.md)
  -   [che Ksetup: mapuser](ksetup-mapuser.md)
  -   [che Ksetup: addkdc](ksetup-addkdc.md)
  -   [che Ksetup: delkdc](ksetup-delkdc.md)
  -   [che Ksetup: addkpasswd](ksetup-addkpasswd.md)
  -   [che Ksetup: delkpasswd](ksetup-delkpasswd.md)
  -   [che Ksetup: Server](ksetup-server.md)
  -   [che Ksetup: setcomputerpassword](ksetup-setcomputerpassword.md)
  -   [che Ksetup: removerealm](ksetup-removerealm.md)
  -   [che Ksetup: dominio](ksetup-domain.md)
  -   [che Ksetup: ChangePassword](ksetup-changepassword.md)
  -   [che Ksetup: listrealmflags](ksetup-listrealmflags.md)
  -   [che Ksetup: setrealmflags](ksetup-setrealmflags.md)
  -   [che Ksetup: addrealmflags](ksetup-addrealmflags.md)
  -   [che Ksetup: delrealmflags](ksetup-delrealmflags.md)
  -   [che Ksetup: dumpstate](ksetup-dumpstate.md)
  -   [che Ksetup: addhosttorealmmap](ksetup-addhosttorealmmap.md)
  -   [che Ksetup: delhosttorealmmap](ksetup-delhosttorealmmap.md)
  -   [che Ksetup: setenctypeattr](ksetup-setenctypeattr.md)
  -   [che Ksetup: getenctypeattr](ksetup-getenctypeattr.md)
  -   [che Ksetup: addenctypeattr](ksetup-addenctypeattr.md)
  -   [che Ksetup: delenctypeattr](ksetup-delenctypeattr.md) 
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L
- [label](label.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  -   [logman create](logman-create.md)
  -   [logman query](logman-query.md)
  -   [logman start & 124; arrestare](logman-start-stop.md)
  -   [logman delete](logman-delete.md)
  -   [logman update](logman-update.md)
  -   [logman Import & 124; esportazione](logman-import-export.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M
- [macfile](macfile.md)
- [makecab](makecab.md)
- [manage-bde](manage-bde.md)
  -   [Manage-bde: stato](manage-bde-status.md)
  -   [Manage-bde: on](manage-bde-on.md)
  -   [Manage-bde: disattivato](manage-bde-off.md)
  -   [Manage-bde: pausa](manage-bde-pause.md)
  -   [Manage-bde: ripresa](manage-bde-resume.md)
  -   [Manage-bde: blocco](manage-bde-lock.md)
  -   [Manage-bde: sblocco](manage-bde-unlock.md)
  -   [Manage-bde: sblocco automatico](manage-bde-autounlock.md)
  -   [Manage-bde: protezioni](manage-bde-protectors.md)
  -   [Manage-bde: TPM](manage-bde-tpm.md)
  -   [Manage-bde: seidentificatore](manage-bde-setidentifier.md)
  -   [Manage-bde: ForceRecovery](manage-bde-forcerecovery.md)
  -   [Manage-bde: ChangePassword](manage-bde-changepassword.md)
  -   [Manage-bde: changepin aggiorna](manage-bde-changepin.md)
  -   [Manage-bde: ChangeKey](manage-bde-changekey.md)
  -   [Manage-bde: pacchetto di pacchetti](manage-bde-keypackage.md)
  -   [Manage-bde: aggiornamento](manage-bde-upgrade.md)
  -   [Manage-bde: WipeFreeSpace](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [Md](Md.md)
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
- [netsh](netsh.md)
- [netstat](netstat.md)
- [Net print](net-print.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  -   [comando di uscita nslookup](nslookup-exit-command.md)
  -   [comando nslookup Finger](nslookup-finger-command.md)
  -   [nslookup help](nslookup-help.md)
  -   [nslookup ls](nslookup-ls.md)
  -   [nslookup lserver](nslookup-lserver.md)
  -   [nslookup root](nslookup-root.md)
  -   [nslookup server](nslookup-server.md)
  -   [nslookup set](nslookup-set.md)
  -   [nslookup set all](nslookup-set-all.md)
  -   [nslookup set class](nslookup-set-class.md)
  -   [nslookup set d2](nslookup-set-d2.md)
  -   [nslookup set debug](nslookup-set-debug.md)
  -   [nslookup set domain](nslookup-set-domain.md)
  -   [nslookup set port](nslookup-set-port.md)
  -   [nslookup set querytype](nslookup-set-querytype.md)
  -   [nslookup set recurse](nslookup-set-recurse.md)
  -   [nslookup set retry](nslookup-set-retry.md)
  -   [nslookup set root](nslookup-set-root.md)
  -   [nslookup set search](nslookup-set-search.md)
  -   [nslookup set srchlist](nslookup-set-srchlist.md)
  -   [nslookup set timeout](nslookup-set-timeout.md)
  -   [nslookup set type](nslookup-set-type.md)
  -   [nslookup set vc](nslookup-set-vc.md)
  -   [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O
-   [openfiles](openfiles.md)

### <a name="p"></a>P
-   [pagefileconfig](pagefileconfig.md)
-   [path](path.md)
-   [pathping](pathping.md)
-   [pause](pause.md)
-   [pbadmin](pbadmin.md)
-   [pentnt](pentnt.md)
-   [perfmon](perfmon.md)
-   [ping](ping.md)
-   [pnpunattend](pnpunattend.md)
-   [pnputil](pnputil.md)
-   [popd](popd.md)
-   [PowerShell](PowerShell.md)
-   [PowerShell_ise](PowerShell_ise.md)
-   [print](print.md)
-   [prncnfg](prncnfg.md)
-   [prndrvr](prndrvr.md)
-   [prnjobs](prnjobs.md)
-   [prnmngr](prnmngr.md)
-   [prnport](prnport.md)
-   [prnqctl](prnqctl.md)
-   [prompt](prompt.md)
-   [pubprn](pubprn.md)
-   [pushd](pushd.md)
-   [pushprinterconnections](pushprinterconnections.md)

### <a name="q"></a>Q
-   [qappsrv](qappsrv.md)
-   [qprocess](qprocess.md)
-   [query](query.md)
-   [quser](quser.md)
-   [qwinsta](qwinsta.md)

### <a name="r"></a>V
- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [reg](reg.md)
  -   [reg Aggiungi](reg-add.md)
  -   [confronto reg](reg-compare.md)
  -   [Copia reg](reg-copy.md)
  -   [reg delete](reg-delete.md)
  -   [esportazione reg](reg-export.md)
  -   [importazione reg](reg-import.md)
  -   [carico reg](reg-load.md)
  -   [query reg](reg-query.md)
  -   [ripristino reg](reg-restore.md)
  -   [reg Salva](reg-save.md)
  -   [Scaricamento reg](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [rem](rem.md)
- [ren](ren.md)
- [rename](rename.md)
- [repair-bde](repair-bde.md)
- [replace](replace.md)
- [reset session](reset-session.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [route_ws2008](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S
- [schtasks](schtasks.md)
- [scwcmd](Scwcmd.md)
  -   [scwcmd: analizza](scwcmd-analyze.md)
  -   [scwcmd: configurare](scwcmd-configure.md)
  -   [scwcmd: Register](scwcmd-register.md) 
  -   [scwcmd: rollback](scwcmd-rollback.md) 
  -   [scwcmd: Transform](scwcmd-transform.md) 
  -   [scwcmd: View](scwcmd-view.md) 
- [secedit](secedit.md)
  -   [secedit: analizza](secedit-analyze.md)
  -   [secedit: configura](secedit-configure.md)
  -   [secedit: Export](secedit-export.md)
  -   [secedit: generaterollback](secedit-generaterollback.md)
  -   [secedit: importazione](secedit-import.md)
  -   [secedit: Validate](secedit-validate.md)
- [serverceipoptin](serverceipoptin.md)
- [Servermanagercmd](Servermanagercmd.md)
- [serverWerOptin](serverweroptin.md)
- [set](set_1.md)
- [setlocal](setlocal.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shutdown](shutdown.md)
- [sort](sort.md)
- [start](start.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)

### <a name="t"></a>Elemento
-   [takeown](takeown.md)
-   [tapicfg](tapicfg.md)
-   [taskkill](taskkill.md)
-   [tasklist](tasklist.md)
-   [tcmsetup](tcmsetup.md)
-   [telnet](telnet.md)
-   [tftp](tftp.md)
-   [time](time.md)
-   [timeout](timeout_1.md)
-   [title](title_1.md)
-   [tlntadmn](tlntadmn.md)
-   [tpmvscmgr](tpmvscmgr.md)
-   [tracerpt](tracerpt_1.md)
-   [tracert](tracert.md)
-   [tree](tree.md)
-   [tscon](tscon.md)
-   [tsdiscon](tsdiscon.md)
-   [tsecimp](tsecimp_1.md)
-   [tskill](tskill.md)
-   [tsprof](tsprof.md)
-   [type](type.md)
-   [typeperf](typeperf.md)
-   [tzutil](tzutil.md)

### <a name="u"></a>U
-   [unlodctr](unlodctr_1.md)

### <a name="v"></a>V
-   [ver](ver.md)
-   [verifier](verifier.md)
-   [verify](verify_1.md)
-   [vol](vol.md)
-   - [vssadmin](vssadmin.md) 

### <a name="w"></a>W
- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  -   [wbadmin Abilita backup](wbadmin-enable-backup.md)
  -   [wbadmin Disabilita backup](wbadmin-disable-backup.md)
  -   [wbadmin avviare il backup](wbadmin-start-backup.md)
  -   [wbadmin Arresta processo](wbadmin-stop-job.md)
  -   [Wbadmin get versions](wbadmin-get-versions.md)
  -   [wbadmin Ottieni elementi](wbadmin-get-items.md)
  -   [wbadmin Avvia ripristino](wbadmin-start-recovery.md)
  -   [stato Wbadmin get](wbadmin-get-status.md)
  -   [comando Wbadmin get Disks](wbadmin-get-disks.md)
  -   [comando Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  -   [comando Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)
  -   [Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  -   [comando Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)
  -   [Wbadmin restore catalog](wbadmin-restore-catalog.md)
  -   [wbadmin Elimina catalogo](wbadmin-delete-catalog.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [wmic](wmic.md)
- [wscript](wscript.md)

### <a name="x"></a>X
-   [xcopy](xcopy.md)
