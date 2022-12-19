
# Technique 1
Get-Package -IncludeWindowsInstaller | ForEach-Object {
    Write-Host $($_.Name);
}


Uninstall-Package "FileZilla 3.61.0"


# Technique 2
$Pkg = Get-Package -Name "CRM.launcher" 
$UninstallCommand = $Pkg.Meta.Attributes['UninstallString']
# "C:\Program Files (x86)\InstallShield Installation Information\{A4F81052-C7EF-454F-9436-9820C125C837}\setup.exe" -runfromtemp -l0x0409  -removeonly --silent
Start-Process -FilePath cmd.exe -ArgumentList '/c', $UninstallCommand -Wait

Get-PackageProvider