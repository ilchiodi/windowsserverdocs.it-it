---
title: Comandi di Windows
description: Comandi di Windows
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/22/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 4cc9bc5c288eb063f333fa598dbb3511f7be5966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820472"
---
# <a name="windows-commands"></a>Comandi di Windows

Tutte le versioni supportate di Windows (server e client) includono un set di comandi della console Win32 predefinito.

Questo set di documentazione sono descritti i comandi di Windows è possibile usare per automatizzare le attività tramite script o strumenti di scripting.

Per trovare informazioni su un comando specifico, nel menu A-Z seguente, fare clic sulla lettera che il comando inizia con e quindi fare clic sul nome di comando.

[A](#BKMK_a) |
[B](#BKMK_b) | 
[C](#BKMK_c) | 
[D](#BKMK_d) | 
[E](#BKMK_e) | 
[F](#BKMK_f) | 
[G](#BKMK_g) | 
[H](#BKMK_h) | 
[I](#BKMK_i) |
[J](#BKMK_j) | 
[K](#BKMK_k) | 
[L](#BKMK_l) | 
[M](#BKMK_m) | 
[N](#BKMK_n) | 
[O](#BKMK_o) | 
[P](#BKMK_p) | 
[Q](#BKMK_q) | 
[R](#BKMK_r) | 
[S](#BKMK_s) | 
[T](#BKMK_t) | 
[U](#BKMK_u) | 
[V](#BKMK_v) | 
[W](#BKMK_w) | 
[X](#BKMK_x) | 
[Y](#BKMK_y) | 
[Z](#BKMK_z)

## <a name="BKMK_PREREQ"></a>Prerequisiti
Le informazioni contenute in questo file PDF si applicano a:

-   Windows Server 2019
-   Windows Server (Canale semestrale)
-   Windows Server 2016
-   Windows Server 2012 R2
-   Windows Server 2012 
-   Windows Server 2008 R2
-   Windows Server 2008
-   Windows 10
-   Windows 8.1

### <a name="BKMK_OVR"></a>Cenni preliminari sulla shell comandi
La shell dei comandi è stato il primo shell incorporata in Windows per automatizzare le attività di routine, ad esempio gestione degli account utente o i backup notturni, con i file batch (bat). Con Windows Script Host è possibile eseguire script più complessi nella shell dei comandi. Per altre informazioni, vedere [cscript](cscript.md) oppure [wscript](wscript.md). È possibile eseguire operazioni in modo più efficiente utilizzando gli script rispetto a quanto possibile tramite l'interfaccia utente. Gli script accettano tutti i comandi che sono disponibili nella riga di comando.

Windows include due shell dei comandi: La shell dei comandi e [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Ogni shell è un programma software che fornisce la comunicazione diretta tra si e il sistema operativo o applicazione, che fornisce un ambiente per automatizzare le operazioni IT.

PowerShell è stato progettato per estendere le funzionalità della shell dei comandi per eseguire i comandi di PowerShell denominati cmdlet. I cmdlet sono simili ai comandi di Windows, ma fornisce un linguaggio di scripting più estendibile. È possibile eseguire i comandi di Windows e i cmdlet di PowerShell in Powershell, ma la shell dei comandi solo possibile eseguire i comandi di Windows e non i cmdlet di PowerShell.

Per più affidabile e aggiornato Windows automazione, è consigliabile usare PowerShell invece di automazione di Windows Script Host per Windows o i comandi di Windows. 
> [!NOTE]
>È anche possibile scaricare e installare [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), la versione open source di PowerShell. 

> [!CAUTION]
> La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare le modifiche seguenti al Registro di sistema, è necessario eseguire il backup tutti i dati importanti nel computer.

> [!NOTE]
> Per abilitare o disabilitare il completamento di nome di file e directory nella shell dei comandi in una sessione di accesso utente o computer, eseguire **regedit.exe** e impostare le opzioni seguenti **valore reg_DWOrd**:
> 
> Processor\completionChar\reg_DWOrd HKEY_LOCAL_MACHINE\Software\Microsoft\Command
> 
> Per impostare il **reg_DWOrd** valore, utilizzare il valore esadecimale del carattere di controllo per una particolare funzione (ad esempio, **0 9** scheda e **08 0** BACKSPACE). Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

## <a name="BKMK_CmdRef"></a>Riferimento della riga di comando A-Z
Per informazioni su un comando di Windows specifico, nel menu A-Z seguente, fare clic sulla lettera che il comando inizia con e quindi fare clic sul nome del comando.

[A](#BKMK_a) |
[B](#BKMK_b) | 
[C](#BKMK_c) | 
[D](#BKMK_d) | 
[E](#BKMK_e) | 
[F](#BKMK_f) | 
[G](#BKMK_g) | 
[H](#BKMK_h) | 
[I](#BKMK_i) |
[J](#BKMK_j) | 
[K](#BKMK_k) | 
[L](#BKMK_l) | 
[M](#BKMK_m) | 
[N](#BKMK_n) | 
[O](#BKMK_o) | 
[P](#BKMK_p) | 
[Q](#BKMK_q) | 
[R](#BKMK_r) | 
[S](#BKMK_s) | 
[T](#BKMK_t) | 
[U](#BKMK_u) | 
[V](#BKMK_v) | 
[W](#BKMK_w) | 
[X](#BKMK_x) | 
[Y](#BKMK_y) | 
[Z](#BKMK_z)

### <a name="BKMK_a"></a>A
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

### <a name="BKMK_b"></a>B
-   [bcdboot](bcdboot.md)
-   [bcdedit](bcdedit.md)
-   [bdehdcfg](bdehdcfg.md)
-   [bitsadmin](bitsadmin.md)
  -   [addfile Bitsadmin](bitsadmin-addfile.md)
  -   [addfileset Bitsadmin](bitsadmin-addfileset.md)
  -   [addfilewithranges Bitsadmin](bitsadmin-addfilewithranges.md)
  -   [Annulla Bitsadmin](bitsadmin-cancel.md)
  -   [Bitsadmin completo](bitsadmin-complete.md)
  -   [creare Bitsadmin](bitsadmin-create.md)
  -   [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  -   [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  -   [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  -   [getcompletiontime Bitsadmin](bitsadmin-getcompletiontime.md)
  -   [getcreationtime Bitsadmin](bitsadmin-getcreationtime.md)
  -   [bitsadmin getdescription](bitsadmin-getdescription.md)
  -   [getdisplayname Bitsadmin](bitsadmin-getdisplayname.md)
  -   [geterror Bitsadmin](bitsadmin-geterror.md)
  -   [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  -   [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  -   [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  -   [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  -   [getmodificationtime Bitsadmin](bitsadmin-getmodificationtime.md)
  -   [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  -   [getnotifycmdline Bitsadmin](bitsadmin-getnotifycmdline.md)
  -   [getnotifyflags Bitsadmin](bitsadmin-getnotifyflags.md)
  -   [getnotifyinterface Bitsadmin](bitsadmin-getnotifyinterface.md)
  -   [bitsadmin getowner](bitsadmin-getowner.md)
  -   [priorità get Bitsadmin](bitsadmin-getpriority.md)
  -   [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  -   [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  -   [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  -   [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  -   [getreplyfilename Bitsadmin](bitsadmin-getreplyfilename.md)
  -   [getreplyprogress Bitsadmin](bitsadmin-getreplyprogress.md)
  -   [bitsadmin getstate](bitsadmin-getstate.md)
  -   [gettype Bitsadmin](bitsadmin-gettype.md)
  -   [Guida Bitsadmin](bitsadmin-help.md)
  -   [info Bitsadmin](bitsadmin-info.md)
  -   [bitsadmin list](bitsadmin-list.md)
  -   [listfiles Bitsadmin](bitsadmin-listfiles.md)
  -   [monitoraggio Bitsadmin](bitsadmin-monitor.md)
  -   [nowrap Bitsadmin](bitsadmin-nowrap.md)
  -   [rawreturn Bitsadmin](bitsadmin-rawreturn.md)
  -   [removecredentials Bitsadmin](bitsadmin-removecredentials.md)
  -   [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  -   [reimpostazione Bitsadmin](bitsadmin-reset.md)
  -   [Riprendi Bitsadmin](bitsadmin-resume.md)
  -   [setaclflag Bitsadmin](bitsadmin-setaclflag.md)
  -   [setcredentials Bitsadmin](bitsadmin-setcredentials.md)
  -   [bitsadmin setdescription](bitsadmin-setdescription.md)
  -   [setdisplayname Bitsadmin](bitsadmin-setdisplayname.md)
  -   [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  -   [setnoprogresstimeout Bitsadmin](bitsadmin-setnoprogresstimeout.md)
  -   [setnotifycmdline Bitsadmin](bitsadmin-setnotifycmdline.md)
  -   [setnotifyflags Bitsadmin](bitsadmin-setnotifyflags.md)
  -   [setpriority Bitsadmin](bitsadmin-setpriority.md)
  -   [setproxysettings Bitsadmin](bitsadmin-setproxysettings.md)
  -   [setreplyfilename Bitsadmin](bitsadmin-setreplyfilename.md)
  -   [Sospendi Bitsadmin](bitsadmin-suspend.md)
  -   [bitsadmin takeownership](bitsadmin-takeownership.md)
  -   [Trasferimento Bitsadmin](bitsadmin-transfer.md)
  -   [bitsadmin util](bitsadmin-util.md)
  -   [incapsulamento Bitsadmin](bitsadmin-wrap.md)
-   [bootcfg](bootcfg.md)
  -   [bootcfg addsw](bootcfg-addsw.md)
  -   [Copia bootcfg](bootcfg-copy.md)
  -   [bootcfg dbg1394](bootcfg-dbg1394.md)
  -   [debug bootcfg](bootcfg-debug.md)  
  -   [BOOTCFG predefinito](bootcfg-default.md)
  -   [bootcfg delete](bootcfg-delete.md)
  -   [BOOTCFG ems](bootcfg-ems.md)
  -   [bootcfg query](bootcfg-query.md)
  -   [bootcfg raw](bootcfg-raw.md)
  -   [bootcfg rmsw](bootcfg-rmsw.md)
  -   [bootcfg timeout](bootcfg-timeout.md)
-   [break](break_1.md)

### <a name="BKMK_c"></a>C
-   [cacls](cacls_1.md)
-   [call](call.md)
-   [cd](cd.md)
-   [certreq](certreq_1.md)
-   [certutil](certutil.md)
-   [change](change.md)
  -   [Modifica accesso](change-logon.md)
  -   [Cambiare porta](change-port.md)
  -   [Modifica utente](change-user.md)
-   [chcp](chcp.md)
-   [chdir](chdir_1.md)
-   [chglogon](chglogon.md)
-   [chgport](chgport.md)
-   [chgusr](chgusr.md)
-   [chkdsk](chkdsk.md)
-   [chkntfs](chkntfs.md)
-   [choice](choice.md)
-   [cipher](cipher.md)
-   [clip](clip.md)
-   [cls](cls.md)
-   [Cmd](Cmd.md)
-   [cmdkey](cmdkey.md)
-   [cmstp](cmstp.md)
-   [color](color.md)
-   [comp](comp.md)
-   [compact](compact.md)
-   [convert](convert.md)
-   [copy](copy.md)
-   [cprofile](cprofile.md)
-   [cscript](cscript.md)

### <a name="BKMK_d"></a>D
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

### <a name="BKMK_e"></a>E
-   [echo](echo.md)
-   [edit](edit.md)
-   [endlocal](endlocal.md)
-   [erase](erase.md)
-   [eventcreate](eventcreate.md)
-   [eventquery](eventquery.md)
-   [eventtriggers](eventtriggers.md)
-   [evntcmd](Evntcmd.md)
-   [exit](exit_2.md)
-   [expand](expand.md)
-   [extract](extract.md)

### <a name="BKMK_f"></a>F
-   [fc](fc.md)
-   [find](find.md)
-   [findstr](findstr.md)
-   [finger](finger.md)
-   [flattemp](flattemp.md)
-   [fondue](fondue.md)
-   [for](for.md)
-   [forfiles](forfiles.md)
-   [format](format.md)
-   [freedisk](freedisk.md)
-   [fsutil](fsutil.md)
  -   [fsutil 8dot3name](fsutil-8dot3name.md) 
  -   [Comportamento fsutil](fsutil-behavior.md) 
  -   [Fsutil file](fsutil-file.md)
  -   [fsutil fsinfo](fsutil-fsinfo.md)
  -   [fsutil hardlink](fsutil-hardlink.md)
  -   [Objectid fsutil](fsutil-objectid.md)
  -   [fsutil quota](fsutil-quota.md)
  -   [Ripristina fsutil](fsutil-repair.md)
  -   [Reparsepoint fsutil](fsutil-reparsepoint.md)
  -   [Fsutil risorse](fsutil-resource.md)
  -   [Fsutil tipo sparse](fsutil-sparse.md)
  -   [La suddivisione in livelli di fsutil](fsutil-tiering.md)
  -   [Fsutil transazione](fsutil-transaction.md)
  -   [fsutil usn](fsutil-usn.md)
  -   [Volume di fsutil](fsutil-volume.md)
  -   [fsutil wim](fsutil-wim.md)
-   [ftp](ftp.md)
-   [ftype](ftype.md)
-   [fveupdate](fveupdate.md)

### <a name="BKMK_g"></a>G
-   [getmac](getmac.md)
-   [gettype](gettype.md)
-   [goto](goto.md)
-   [gpfixup](gpfixup.md)
-   [gpresult](gpresult.md)
-   [gpupdate](gpupdate.md)
-   [graftabl](graftabl.md)

### <a name="BKMK_h"></a>H
-   [help](help.md)
-   [helpctr](helpctr.md)
-   [hostname](hostname.md)

### <a name="BKMK_i"></a>I
-   [icacls](icacls.md)
-   [if](if.md)
-   [inuse](inuse.md)
-   [ipconfig](ipconfig.md)
-   [ipxroute](ipxroute.md)
-   [irftp](irftp.md)

### <a name="BKMK_j"></a>J
-   [jetpack](jetpack.md)

### <a name="BKMK_k"></a>K
-   [klist](klist.md)
-   [ksetup](ksetup.md)
  -   [ksetup:setrealm](ksetup-setrealm.md)
  -   [ksetup:mapuser](ksetup-mapuser.md)
  -   [ksetup:addkdc](ksetup-addkdc.md)
  -   [ksetup:delkdc](ksetup-delkdc.md)
  -   [ksetup:addkpasswd](ksetup-addkpasswd.md)
  -   [ksetup:delkpasswd](ksetup-delkpasswd.md)
  -   [ksetup:server](ksetup-server.md)
  -   [ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
  -   [ksetup:removerealm](ksetup-removerealm.md)
  -   [ksetup:domain](ksetup-domain.md)
  -   [ksetup:changepassword](ksetup-changepassword.md)
  -   [ksetup:listrealmflags](ksetup-listrealmflags.md)
  -   [ksetup:setrealmflags](ksetup-setrealmflags.md)
  -   [ksetup:addrealmflags](ksetup-addrealmflags.md)
  -   [ksetup:delrealmflags](ksetup-delrealmflags.md)
  -   [ksetup:dumpstate](ksetup-dumpstate.md)
  -   [ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
  -   [ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
  -   [ksetup:setenctypeattr](ksetup-setenctypeattr.md)
  -   [ksetup:getenctypeattr](ksetup-getenctypeattr.md)
  -   [ksetup:addenctypeattr](ksetup-addenctypeattr.md)
  -   [ksetup:delenctypeattr](ksetup-delenctypeattr.md) 
-   [ktmutil](ktmutil.md)
-   [ktpass](ktpass.md)

### <a name="BKMK_l"></a>L
-   [label](label.md)
-   [lodctr](lodctr.md)
-   [logman](logman.md)
  -   [creare Logman](logman-create.md)
  -   [Logman query](logman-query.md)
  -   [logman start &124; stop](logman-start-stop.md)
  -   [Logman delete](logman-delete.md)
  -   [Logman update](logman-update.md)
  -   [logman import &124; export](logman-import-export.md)
-   [logoff](logoff.md)
-   [lpq](lpq.md)
-   [lpr](lpr.md)

### <a name="BKMK_m"></a>M
-   [macfile](macfile.md)
-   [makecab](makecab.md)
-   [manage-bde](manage-bde.md)
  -   [manage-bde: status](manage-bde-status.md)
  -   [manage-bde: on](manage-bde-on.md)
  -   [manage-bde: off](manage-bde-off.md)
  -   [manage-bde: pause](manage-bde-pause.md)
  -   [manage-bde: resume](manage-bde-resume.md)
  -   [manage-bde: lock](manage-bde-lock.md)
  -   [manage-bde: unlock](manage-bde-unlock.md)
  -   [manage-bde: autounlock](manage-bde-autounlock.md)
  -   [manage-bde: protectors](manage-bde-protectors.md)
  -   [manage-bde: tpm](manage-bde-tpm.md)
  -   [manage-bde: setidentifier](manage-bde-setidentifier.md)
  -   [manage-bde: ForceRecovery](manage-bde-forcerecovery.md)
  -   [manage-bde: changepassword](manage-bde-changepassword.md)
  -   [manage-bde: changepin](manage-bde-changepin.md)
  -   [manage-bde: changekey](manage-bde-changekey.md)
  -   [manage-bde: KeyPackage](manage-bde-keypackage.md)
  -   [manage-bde: upgrade](manage-bde-upgrade.md)
  -   [manage-bde: WipeFreeSpace](manage-bde-wipefreespace.md)
-   [mapadmin](mapadmin.md)
-   [Md](Md.md)
-   [mkdir](mkdir.md)
-   [mklink](mklink.md)
-   [mmc](mmc.md)
-   [mode](mode.md)
-   [more](more.md)
-   [mount](mount.md)
-   [mountvol](mountvol.md)
-   [move](move.md)
-   [mqbkup](mqbkup.md)
-   [mqsvc](mqsvc.md)
-   [mqtgsvc](mqtgsvc.md)
-   [msdt](msdt.md)
-   [msg](msg.md)
-   [msiexec](msiexec.md)
-   [msinfo32](msinfo32.md)
-   [mstsc](mstsc.md)

### <a name="BKMK_n"></a>N
-   [nbtstat](nbtstat.md)
-   [netcfg](netcfg.md)
-   [netsh](netsh.md)
-   [netstat](netstat.md)
-   [Stampa di rete](net-print.md)
-   [nfsadmin](nfsadmin.md)
-   [nfsshare](nfsshare.md)
-   [nfsstat](nfsstat.md)
-   [nlbmgr](nlbmgr.md)
-   [nslookup](nslookup.md)
  -   [comando uscita nslookup](nslookup-exit-command.md)
  -   [comando nslookup con un dito](nslookup-finger-command.md)
  -   [Guida di nslookup](nslookup-help.md)
  -   [ls nslookup](nslookup-ls.md)
  -   [nslookup lserver](nslookup-lserver.md)
  -   [Nslookup radice](nslookup-root.md)
  -   [server di nslookup](nslookup-server.md)
  -   [set nslookup](nslookup-set.md)
  -   [Imposta tutti nslookup](nslookup-set-all.md)
  -   [classe set nslookup](nslookup-set-class.md)
  -   [d2 set nslookup](nslookup-set-d2.md)
  -   [Impostare debug nslookup](nslookup-set-debug.md)
  -   [dominio set nslookup](nslookup-set-domain.md)
  -   [Nslookup impostare porta](nslookup-set-port.md)
  -   [nslookup set querytype](nslookup-set-querytype.md)
  -   [recurse set nslookup](nslookup-set-recurse.md)
  -   [nslookup set tentativi](nslookup-set-retry.md)
  -   [Nslookup Imposta radice](nslookup-set-root.md)
  -   [ricerca set nslookup](nslookup-set-search.md)
  -   [srchlist set nslookup](nslookup-set-srchlist.md)
  -   [Nslookup impostare timeout](nslookup-set-timeout.md)
  -   [tipo di set nslookup](nslookup-set-type.md)
  -   [Nslookup impostare vc](nslookup-set-vc.md)
  -   [visualizzazione di nslookup](nslookup-view.md)
-   [ntbackup](ntbackup.md)
-   [ntcmdprompt](ntcmdprompt.md)
-   [ntfrsutl](ntfrsutl.md)

### <a name="BKMK_o"></a>O
-   [openfiles](openfiles.md)

### <a name="BKMK_p"></a>P
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

### <a name="BKMK_q"></a>Q
-   [qappsrv](qappsrv.md)
-   [qprocess](qprocess.md)
-   [query](query.md)
-   [quser](quser.md)
-   [qwinsta](qwinsta.md)

### <a name="BKMK_r"></a>R
-   [rcp](rcp.md)
-   [rd](rd.md)
-   [rdpsign](rdpsign.md)
-   [recover](recover.md)
-   [reg](reg.md)
  -   [REG aggiungere](reg-add.md)
  -   [Confronto Reg](reg-compare.md)
  -   [Copia Reg](reg-copy.md)
  -   [Reg delete](reg-delete.md)
  -   [Reg export](reg-export.md)
  -   [reg import](reg-import.md)
  -   [Carico Reg](reg-load.md)
  -   [reg query](reg-query.md)
  -   [Ripristino Reg](reg-restore.md)
  -   [reg save](reg-save.md)
  -   [Reg unload](reg-unload.md)
-   [regini](regini.md)
-   [regsvr32](regsvr32.md)
-   [relog](relog.md)
-   [rem](rem.md)
-   [ren](ren.md)
-   [rename](rename.md)
-   [repair-bde](repair-bde.md)
-   [replace](replace.md)
-   [Reimposta sessione](reset-session.md)
-   [rexec](rexec.md)
-   [risetup](risetup.md)
-   [rmdir](rmdir.md)
-   [robocopy](robocopy.md)
-   [route_ws2008](route_ws2008.md)
-   [rpcinfo](rpcinfo.md)
-   [rpcping](rpcping.md)
-   [rsh](rsh.md)
-   [rundll32](rundll32.md)
-   [rwinsta](rwinsta.md)

### <a name="BKMK_s"></a>S
-   [schtasks](schtasks.md)
-   [scwcmd](Scwcmd.md)
  -   [scwcmd: analyze](scwcmd-analyze.md)
  -   [scwcmd: configure](scwcmd-configure.md)
  -   [scwcmd: register](scwcmd-register.md) 
  -   [scwcmd: rollback](scwcmd-rollback.md) 
  -   [scwcmd: transform](scwcmd-transform.md) 
  -   [scwcmd: view](scwcmd-view.md) 
-   [secedit](secedit.md)
  -   [secedit:analyze](secedit-analyze.md)
  -   [secedit:configure](secedit-configure.md)
  -   [secedit:export](secedit-export.md)
  -   [secedit:generaterollback](secedit-generaterollback.md)
  -   [secedit:import](secedit-import.md)
  -   [secedit:validate](secedit-validate.md)
-   [serverceipoptin](serverceipoptin.md)
-   [Servermanagercmd](Servermanagercmd.md)
-   [serverweroptin](serverweroptin.md)
-   [set](set_1.md)
-   [setlocal](setlocal.md)
-   [setx](setx.md)
-   [sfc](sfc.md)
-   [shadow](shadow.md)
-   [shift](shift.md)
-   [showmount](showmount.md)
-   [shutdown](shutdown.md)
-   [sort](sort.md)
-   [start](start.md)
-   [subst](subst.md)
-   [sxstrace](sxstrace.md)
-   [sysocmgr](sysocmgr.md)
-   [systeminfo](systeminfo.md)

### <a name="BKMK_t"></a>T
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

### <a name="BKMK_u"></a>U
-   [unlodctr](unlodctr_1.md)

### <a name="BKMK_v"></a>V
-   [ver](ver.md)
-   [verifier](verifier.md)
-   [verify](verify_1.md)
-   [vol](vol.md)
-   [vssadmin](vssadmin.md)- 

### <a name="BKMK_w"></a>W
-   [waitfor](waitfor.md)
-   [wbadmin](wbadmin.md)
  -   [Abilita backup Wbadmin](wbadmin-enable-backup.md)
  -   [disabilitare il backup Wbadmin](wbadmin-disable-backup.md)
  -   [Comando Wbadmin start backup](wbadmin-start-backup.md)
  -   [Processo di arresto Wbadmin](wbadmin-stop-job.md)
  -   [Wbadmin get versioni](wbadmin-get-versions.md)
  -   [Wbadmin get elementi](wbadmin-get-items.md)
  -   [Comando Wbadmin start recovery](wbadmin-start-recovery.md)
  -   [Comando Wbadmin ottenere lo stato](wbadmin-get-status.md)
  -   [Wbadmin get dischi](wbadmin-get-disks.md)
  -   [wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  -   [Comando Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)
  -   [comando Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  -   [Comando Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)
  -   [Catalogo di ripristino Wbadmin](wbadmin-restore-catalog.md)
  -   [Comando Wbadmin delete catalogo](wbadmin-delete-catalog.md)
-   [wdsutil](wdsutil.md)
-   [wecutil](wecutil.md)
-   [wevtutil](wevtutil.md)
-   [where](where_1.md)
-   [whoami](whoami.md)
-   [winnt](winnt.md)
-   [winnt32](winnt32.md)
-   [winpop](winpop.md)
-   [winrs](winrs.md)
-   [wlbs](wlbs_1.md)
-   [wmic](wmic.md)
-   [wscript](wscript.md)

### <a name="BKMK_x"></a>X
-   [xcopy](xcopy.md)
