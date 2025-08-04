
WSL Setup:
----------
wsl --install

'wsl.exe -d Ubuntu'

Ubuntu içinden Windows dosyalarına:
/mnt/c/Users/kullanici_adiniz/Desktop

Windows’tan Ubuntu içindeki dosyalara erişim:
Windows Dosya Gezgini’ne şunu yaz:
\\wsl$\Ubuntu\

sudo apt update
sudo apt install git

wsl --list --verbose
wsl --shutdown