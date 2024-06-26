# go-winapi
This module contains functions to use Windows APIs that are missing in the Windows package from the standard library.

I left to the user the power to choose when to load which DLL and get the addresses to functions. You may need them in your code, who knows.
For this reason, the module requires you to pass to the functions windows.LazyProc structuctures, initialized with the addresses of the functions exported by the DLLs.

Load the DLL and get the function address:

```
var (
	kernel32                     = windows.NewLazySystemDLL("kernel32.dll")
	ntdll                        = windows.NewLazySystemDLL("ntdll.dll")
	procVirtualAllocEx           = kernel32.NewProc("VirtualAllocEx")
	procHeapAlloc                = kernel32.NewProc("HeapAlloc")
	procCreateThread             = kernel32.NewProc("CreateThread")
	procCreateRemoteThread       = kernel32.NewProc("CreateRemoteThread")
	procCreateToolhelp32Snapshot = kernel32.NewProc("CreateToolhelp32Snapshot")
	procRtlIpv4StringToAddressA  = ntdll.NewProc("RtlIpv4StringToAddressA")
)
```
### Usage example:

```
package main

import (
	winapi "github.com/DoctorDolorem/winapis"
)

var kernel32 = windows.NewLazySystemDLL("kernel32.dll")
var procVirtualAllocEx = kernel32.NewProc("VirtualAllocEx")

func main(){

pAddress, err := winapi.VirtualAllocEx(procVirtualAllocEx, hProcess, 0, 0, windows.MEM_COMMIT | windows.MEM_RESERVE, windows.PAGE_READWRITE)

}
```
