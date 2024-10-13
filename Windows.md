net user Visitor /add /active:yes
net user Visitor *
net localgroup users Visitor /delete
net localgroup guests Visitor /add


XCOPY Command Reference:
************************

Run the command prompt as administrator.

1. copy folders and subfolders and contents
-------------------------------------------

xcopy y:\AppStore\* A:\ /E /H /C /I
xcopy y:\Backup\* B:\ /E /H /C /I

/E – Copy subdirectories, including any empty ones.
/H - Copy files with hidden and system file attributes.
/C - Continue copying even if an error occurs.
/I - If in doubt, always assume the destination is a folder. e.g. when the destination does not exist.

2. copy folders and subfolders without files
--------------------------------------------

Xcopy Source Destination /T /E

/T - Copy the subdirectory structure, but not the files.
/E - Copy subdirectories, including any empty ones.

3. copy folders and subfolders with NTFS and Share permission
-------------------------------------------------------------

Xcopy Source Destination /O /X /E /H /K

/E - Copy folders and subfolders, including empty ones.
/H - Copy hidden and system files also.
/K - Copy attributes. Typically, Xcopy resets read-only attributes.
/O - Copy file ownership and ACL information.
/X - Copy file audit settings (implies /O).


https://ss64.com/nt/xcopy.html