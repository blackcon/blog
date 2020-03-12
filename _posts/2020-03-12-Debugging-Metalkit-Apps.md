---
title: "vmware의 metalkit 디버깅"
date: 2021-03-12 11:55:00 +0900
categories: vmware SVGA
---

Debugging Metalkit Apps
-----------------------

1. .vmx 파일 수정
vmware의 가상 이미지 설정파일인 .vmx를 editor로 열어서 아래의 내용을 추가해준다.
    debugStub.listen.guest32 = "TRUE"
    debugStub.hideBreakpoints = "TRUE"
    monitor.debugOnStartGuest32 = "TRUE"
    
2. 가상 이미지 실행
가상 이미지를 실행시키면 bios에 내포되어 있는 실행파일(ELF)이 실행되기 전에 화면이 멈춰있게 된다.
(.vmx 파일에 디버깅 옵션을 넣어주었기 때문)

3. remote attach
`조건`: bios에 포함되어 있는 ELF 바이너리를 가지고 있어야 한다.
    micah@micah-64:~/metalkit/examples/apm-test$ gdb -q apm-test.elf
    No symbol table is loaded.  Use the "file" command.
    Using host libthread_db library "/lib/libthread_db.so.1".
    (gdb) set arch i386
    The target architecture is assumed to be i386
    (gdb) target remote localhost:8832
    Remote debugging using localhost:8832
    [New Thread 1]
    0x000ffff0 in ?? ()
    (gdb) break main
    Breakpoint 1 at 0x100ba2: file main.c, line 11.
    (gdb) cont
    Continuing.
    
 9. Reference
 - [github - vmware-svga:debugging.txt](https://github.com/prepare/vmware-svga/blob/master/doc/debugging.txt)
 
