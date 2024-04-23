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

for windows 10
# Forcefully update the Group Policy
```
gpupdate /force
```
# Check IP configuration
```
ipconfig
```
# Display network statistics (similar to netstat)
```
Get-NetTCPConnection
```
# Perform a traceroute
```
Test-NetConnection -TraceRoute <destination>
```
# Open the Group Policy Management Console
```
gpmc.msc
```
# Open the Run dialog
```
Windows key + R
```
# Open the Power User menu
```
Windows key + X
```
# Display the hostname
```
hostname
```
# Display total physical memory in MB
```
(Get-CimInstance -ClassName Win32_ComputerSystem).TotalPhysicalMemory / 1MB
```
# Display total physical memory in GB
```
(Get-CimInstance -ClassName Win32_ComputerSystem).TotalPhysicalMemory / 1GB
```
# Display HDD storage information
```
Get-WmiObject Win32_LogicalDisk | Select-Object DeviceID, VolumeName, Size, FreeSpace
```
# Display HDD storage information in GB
```
Get-WmiObject Win32_LogicalDisk | 
    Select-Object DeviceID, VolumeName, @{Name="Size(GB)";Expression={[math]::Round($_.Size / 1GB, 2)}}, @{Name="FreeSpace(GB)";Expression={[math]::Round($_.FreeSpace / 1GB, 2)}}
```
# Display the operating system name
```
systeminfo | find "OS Name"
```
# Display the processor information
```
systeminfo | find "Processor(s)"
```
# Display processor name and maximum clock speed
```
Get-WmiObject Win32_Processor | Select-Object Name, MaxClockSpeed
```
# Display IP address
```
Get-NetIPAddress -AddressFamily IPv4 | Select-Object IPAddress
```
# Determine if disk is SSD or HDD
```
Get-WmiObject Win32_DiskDrive | 
    Select-Object DeviceID, MediaType, Model, SerialNumber, 
        @{Name="Size(GB)";Expression={[math]::Round($_.Size / 1GB, 2)}}, 
        @{Name="IsSSD";Expression={($_.MediaType -eq "SSD") -or ($_.InterfaceType -match "SolidState")}}
```
# Check activation status
```
slmgr /xpr
slmgr /dlv
slmgr /dli
slmgr /ato
```
# Get VPN connections
```
Get-VpnConnection | Select-Object -Property Name, ConnectionState
```
