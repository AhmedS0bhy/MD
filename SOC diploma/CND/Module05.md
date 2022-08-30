# Windows Architecture & security concerns 
### Two diffrent modes for operation 
- user mode 
- kernel mode

## Windows Security components 
- Security reference monitor (SRM)  
    - controlling access of user to a resource 
    - checks the privileges of the user 
    - %SystemRoot%\System32\Ntoskrnl.exe
- Local Security Authority Subsystem (LSASS)
    - local security authority 
    - implementating local security policies
    - issues security tokens to accounts
    - policy database 
    - LSASS database can be located in HKLM\SECURITY 
- SAM 
    - responsible for managing the database that contiaing the user names and groups 
    - %SystemRoot%\System32\Samsrv.dll
    - Stores and hashes logon credentials of local users and group accounts 
    - SAM database is located in WIndows registery in C:\Windows\System32\config
- Authontication Packages & Winlogon     
