# sv_update_winddows
Comando actualizacion windows
# Instalacion
```Install-Module PSWindowsUpdate -Force```

# Bloqueo 
```Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass```

# Update 
```Install-WindowsUpdate -AcceptAll -IgnoreReboot```

# Evidencia 
```Write-Host "HOSTNAME: $(hostname)"; Write-Host "IP: $((Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike '127.*' -and $_.InterfaceAlias -notlike '*Loopback*'}).IPAddress)"; Write-Host "`nULTIMOS PARCHES:"; Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 10 HotFixID,Description,InstalledOn```

# Proxie 
```$env:http_proxy="http://squidadmin:`$1Val32022`$qu1D@172.16.100.122:4128/"; $env:https_proxy="http://squidadmin:`$1Val32022`$qu1D@172.16.100.122:4128/"; $env:no_proxy="127.0.0.1,localhost"; $env:NO_PROXY="127.0.0.1,localhost"```
 

