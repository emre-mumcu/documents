
Get-ChildItem -Path "C:\Your\Target\Folder" -File | ForEach-Object { $new=$_.Name -replace '[^\x00-\x7F]','_'; if($_.Name -ne $new){Rename-Item $_.FullName $new} }


Get-ChildItem -Path . -File | ForEach-Object { $new=$_.Name -replace '[^\x00-\x7F]','_'; if($_.Name -ne $new){Rename-Item $_.FullName $new} }


--One-liner for current folder (no subfolders):
Get-ChildItem -Path . -File | ForEach-Object { $new=$_.Name -replace '[^\x00-\x7F]','_'; if($_.Name -ne $new){Rename-Item $_.FullName $new; "$($_.FullName) -> $new" | Out-File -FilePath .\rename_log.txt -Append } }

-- One-liner for current folder + all subfolders (files + folders):
Get-ChildItem -Path . -Recurse | ForEach-Object { $new=$_.Name -replace '[^\x00-\x7F]','_'; if($_.Name -ne $new){Rename-Item $_.FullName $new; "$($_.FullName) -> $new" | Out-File -FilePath .\rename_log.txt -Append } }


-- Here’s the robust one-liner for current folder + subfolders, with logging and conflict handling:

Get-ChildItem -Path . -File | ForEach-Object {
    $new = $_.Name -replace '[^\x00-\x7F]', '_'
    if ($_.Name -ne $new) {
        $dir = $_.DirectoryName
        $base = [System.IO.Path]::GetFileNameWithoutExtension($new)
        $ext  = [System.IO.Path]::GetExtension($new)
        $final = $new
        $i = 1
        while (Test-Path (Join-Path $dir $final)) {
            $final = "${base}_$i$ext"
            $i++
        }
        Rename-Item $_.FullName $final
        "$($_.FullName) -> $final" | Out-File -FilePath .\rename_log.txt -Append
    }
}


-- Here’s the fixed script with logging, conflict handling, and safe renaming for any file/folder name:

Get-ChildItem -Path . -File | ForEach-Object {
    $new = $_.Name -replace '[^\x00-\x7F]', '_'
    if ($_.Name -ne $new) {
        $dir = $_.DirectoryName
        $base = [System.IO.Path]::GetFileNameWithoutExtension($new)
        $ext  = [System.IO.Path]::GetExtension($new)
        $final = $new
        $i = 1
        while (Test-Path -LiteralPath (Join-Path $dir $final)) {
            $final = "${base}_$i$ext"
            $i++
        }
        Rename-Item -LiteralPath $_.FullName -NewName $final
        "$($_.FullName) -> $(Join-Path $dir $final)" | Out-File -FilePath .\rename_log.txt -Append
    }
}















# Set the target folder (change this to your folder path)
$FolderPath = "C:\Users\Emre\Desktop\Yeni klasör"

# Get all files in the folder (and subfolders if you want, use -Recurse)
Get-ChildItem -Path $FolderPath -File | ForEach-Object {
    $OldName = $_.Name
    # Replace non-ASCII characters with underscore
    $NewName = ($OldName -replace '[^\x00-\x7F]', '_')

    # Only rename if different
    if ($OldName -ne $NewName) {
        $NewPath = Join-Path $_.DirectoryName $NewName
        Rename-Item -Path $_.FullName -NewName $NewName
        Write-Host "Renamed: $OldName -> $NewName"
    }
}