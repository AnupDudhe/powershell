# powershell
# windows-cli
```
gpupdate        -- forcefully update the policy
gpupdate /force -- forcefully update the policy
gpmc.ms
ipconfig -- to checkip
netstat --
traceroute -- 
```
windows + R
windows + X
-----------------------------------------------------------------------------
```
hostname
```
-------------------------------------------------------------------------------
RAM DATA (Memory)
```
systeminfo | find "Total Physical Memory" - in MB
```
```
(Get-CimInstance -ClassName Win32_ComputerSystem).TotalPhysicalMemory / 1GB - in GBs
```
-----------------------------------------------------------------------------------
HDD (Storage)
```
Get-WmiObject Win32_LogicalDisk | Select-Object DeviceID, VolumeName, Size, FreeSpace
```
OR
```
Get-WmiObject Win32_LogicalDisk | 
    Select-Object DeviceID, VolumeName, @{Name="Size(GB)";Expression={[math]::Round($_.Size / 1GB, 2)}}, @{Name="FreeSpace(GB)";Expression={[math]::Round($_.FreeSpace / 1GB, 2)}}
```
OR

in human readable format (GB)
```
Get-WmiObject Win32_LogicalDisk | 
    Select-Object DeviceID, VolumeName, @{Name="Size(GB)";Expression={[math]::Round($_.Size / 1GB, 2)}}, @{Name="FreeSpace(GB)";Expression={[math]::Round($_.FreeSpace / 1GB, 2)}}
```
OR
```
Get-WmiObject Win32_LogicalDisk | 
    Select-Object DeviceID, VolumeName, 
        @{Name="Size(GB)"; Expression={[math]::Round($_.Size / 1GB, 2)}}, 
        @{Name="FreeSpace(GB)"; Expression={[math]::Round($_.FreeSpace / 1GB, 2)}}
```
------------------------------------------------------------------------------
OS 
```
systeminfo | find "OS Name"
```
-----------------------------------------------------------------------------
Processor
```
systeminfo | find "Processor(s)"
```
better command
```
Get-WmiObject Win32_Processor | Select-Object Name, MaxClockSpeed
```
```
Get-WmiObject Win32_Processor | Select-Object @{Name="Processor"; Expression={$_.Name}}, @{Name="Max Clock Speed (GHz)"; Expression={$_.MaxClockSpeed / 1000}}
```
----------------------------------------------------------------------
Ip Address
```
ipconfig | findstr IPv4
```
```
Get-NetIPAddress -AddressFamily IPv4 | Select-Object IPAddress
```
---------------------------------------------------------------------------

SDD 0R HDD
COMMAND
```
Get-WmiObject Win32_DiskDrive | Select-Object DeviceID, MediaType
```


SCRIPT
```
Get-WmiObject Win32_DiskDrive | ForEach-Object {
    $disk = $_
    $diskMediaType = $disk.MediaType
    $diskModel = $disk.Model

    Get-WmiObject Win32_PhysicalMedia | Where-Object { $_.Tag -eq $disk.DeviceID } | ForEach-Object {
        $physicalMedia = $_
        $serialNumber = $physicalMedia.SerialNumber
        $interfaceType = $physicalMedia.InterfaceType
        $size = [math]::Round($physicalMedia.Capacity / 1GB, 2)
        
        # Check if SSD based on MediaType or InterfaceType
        $isSSD = ($diskMediaType -eq "SSD" -or $interfaceType -match "SolidState") -as [int]

        [PSCustomObject]@{
            DeviceID = $disk.DeviceID
            Model = $diskModel
            SerialNumber = $serialNumber
            Size_GB = $size
            MediaType = $diskMediaType
            InterfaceType = $interfaceType
            IsSSD = $isSSD
        }
    }
}
```

--------------------------------------------------------------------------------------
check activation keys
```
slmgr.vbs /xpr
```
slmgr.vbs /dlv
```
slmgr.vbs /dli
```
```
slmgr.vbs /ato
```
---------------------------------------------------------------------------------------
Get vpn connections
```
Get-VpnConnection | Select-Object -Property Name, ConnectionState
```
