# sv_update_winddows
Comando actualizacion windows
# Instalacion
```Install-Module PSWindowsUpdate -Force```

# Importar modulo 
```Import-Module PSWindowsUpdate```

# Bloqueo 
```Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass```

# Update 
```Install-WindowsUpdate -AcceptAll -IgnoreReboot```

```Install-WindowsUpdate -MicrosoftUpdate -AcceptAll -IgnoreReboot```

# Evidencia 
```Write-Host "HOSTNAME: $(hostname)"; Write-Host "IP: $((Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike '127.*' -and $_.InterfaceAlias -notlike '*Loopback*'}).IPAddress)"; Write-Host "`nULTIMOS PARCHES:"; Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 10 HotFixID,Description,InstalledOn```

# Proxie 
```$env:http_proxy="http://squidadmin:`$1Val32022`$qu1D@172.16.100.122:4128/"; $env:https_proxy="http://squidadmin:`$1Val32022`$qu1D@172.16.100.122:4128/"; $env:no_proxy="127.0.0.1,localhost"; $env:NO_PROXY="127.0.0.1,localhost"```

# WSUS Verificar
```
$WU = 'HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate'
Get-ItemProperty $WU -ErrorAction SilentlyContinue |
Select-Object WUServer,WUStatusServer,TargetGroup,TargetGroupEnabled
Get-ItemProperty "$WU\AU" -ErrorAction SilentlyContinue |
Select-Object UseWUServer
```
# WSUS Quitar
```
$WU = 'HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate'
Remove-ItemProperty $WU -Name WUServer -ErrorAction SilentlyContinue
Remove-ItemProperty $WU -Name WUStatusServer -ErrorAction SilentlyContinue
if (Test-Path "$WU\AU") {
    Set-ItemProperty "$WU\AU" -Name UseWUServer -Value 0 -Type DWord
}
```
# Reiniciar WSUS 
```Restart-Service wuauserv -Force```

# Instalar y preparar el módulo
```
Set-ExecutionPolicy Unrestricted -Force -ErrorAction SilentlyContinue
Install-Module -Name PSWindowsUpdate -Force
Import-Module PSWindowsUpdate
```
# Registrar
```
Add-WUServiceManager -MicrosoftUpdate -Confirm:$false
```

# Despues de desinstalar el WSUS
```
Stop-Service wuauserv -Force
Stop-Service bits -Force
Stop-Service cryptsvc -Force
Rename-Item C:\Windows\SoftwareDistribution SoftwareDistribution.old -ErrorAction SilentlyContinue
Rename-Item C:\Windows\System32\catroot2 catroot2.old -ErrorAction SilentlyContinue
Rename-Item C:\Windows\SoftwareDistribution SoftwareDistribution.old -ErrorAction SilentlyContinue
Rename-Item C:\Windows\System32\catroot2 catroot2.old -ErrorAction SilentlyContinue
Start-Service wuauserv
Start-Service bits
Start-Service cryptsvc
```

```
Stop-Service -Name wuauserv, bits, cryptsvc, msiserver -Force -ErrorAction SilentlyContinue=
$Process = Get-CimInstance Win32_Process | Where-Object {$_.ExecutablePath -like "*SoftwareDistribution*"}
if ($Process) { Stop-Process -Id $Process.ProcessId -Force }
Rename-Item -Path "C:\Windows\SoftwareDistribution" -NewName "SoftwareDistribution.old" -Force
Rename-Item -Path "C:\Windows\System32\catroot2" -NewName "catroot2.old" -Force
Start-Service -Name wuauserv, bits, cryptsvc, msiserver -ErrorAction SilentlyContinue
```
