# AD_Snapshot_Functions
This repo provides a fork of the AD_Snapshot_Functions PowerShell cmdlets, released by [Ahsley McGlone](https://github.com/GoateePFE) back in 2014 and now part of his [TechNetGalleryArchive](https://github.com/GoateePFE/TechNetGalleryArchive). You may find the initial release [here](https://gallery.technet.microsoft.com/Active-Directory-Attribute-0f815689), as long as the TechNet Gallery is still alive. Even today, I regard the AD-Snapshot-Cmdlets to be a MUST-HAVE for every Active Directory administrator to deal with attribute recovery using PowerShell.

Unfortunately, the original and unmodified script, which you may find [here](https://github.com/cknermann/AD_Snapshot_Functions/blob/master/AD_Snapshot_Functions_en.ps1), would only run on an English system, because the cmdlet *Mount-ADDatabase* parses the output of ntdsutil.exe and expects it to return an answer like "Snapshot ... **mounted as** C:\$SNAP_...\_VOLUMEC$\". On a German system though ntdsutil.exe would return something like "Der Snapshot ... **wird als** C:\$SNAP_..._VOLUMEC$\ **bereitgestellt.**" and *Mount-ADDatabase* would fail. So all I had to do, was to change line 167 of the script:

```sh
$MountPath = (($Mount | Select-String -SimpleMatch 'wird als' | Where-Object {$_ -like "*VOLUME$($DITPathDrive)$*"}) -split 'wird als')[-1].Trim().TrimEnd(" bereitgestellt.")
```

The [modified script](https://github.com/cknermann/AD_Snapshot_Functions/blob/master/AD_Snapshot_Functions_de.ps1) now executes on a German server system. It comes "as-it-is" without any warranties or support. Please feel free to further adapt it to suit your needs.