# Normalization Rules (MS Teams)
This small and simple script was used to bulk provision **normalization rules** for MS Teams dial plan.<br>
The `.csv` file needed was uploaded and it has dummy data, please download it and fill it out with your own data.<br>
```powershell
Clear

$CSV = Import-Csv -Path "C:\Users\user\Documents\data.csv"

$i = 0;

ForEach ($Row in $CSV) {

                    $POS1 = $($Row.'ARCO');
                    $POS2 = $($Row.'FPNU');
                    $POS3 = $($Row.'SPNU');
                    $TENUM = "($POS1)$POS2-$POS3"
                                        
                    Write-Host "Working with $TENUM" 

                    $DID_RAM = $($Row.'DID');
                    $Ext_RAM = $($Row.'Ext');
                    $string = "`$1"
                   
                                                          
                    $noru=New-CsVoiceNormalizationRule -Parent Global -Description 'extensions dialing' -Name "NR $Ext_RAM" -Pattern "^($Ext_RAM\d*)$" -Translation "+1$DID_RAM;ext=$string" -IsInternalExtension $false -InMemory
                    Set-CsTenantDialPlan -Identity Global -NormalizationRules @{add=$noru}
                    
                    $i += +1;

                    write-Host " $i Normalizations rules have been processed.." -ForegroundColor Yellow -BackgroundColor DarkGreen
                    Write-Host ""

}
```
## Normalization Rules use case
Let's say you are moving users from and old PBX to MS Teams and your users are used to dial by extension (internally)<br>
then you will need `Normalization rules` to translate each extension number into its correspondent E.164 formated number<br>
![e164](https://github.com/danielurra/ms-teams-normalization-rules/assets/51704179/32a7cb34-1b01-492a-b85b-d4ea9d33faa8)<br>
remember that MS Teams makes use of the E.164 format.<br>
## Powershell script execution completed
![Normalization-rules-script-running](https://github.com/danielurra/ms-teams-normalization-rules/assets/51704179/8525e6f6-b203-46bd-a054-5c71026ee8d2)<br>
## MS Teams Adminstration Center - Normalization Rules successfully created
![Normalization-rules-admin-center](https://github.com/danielurra/ms-teams-normalization-rules/assets/51704179/47c2d1f8-a880-47b8-a63a-4cf4024e3416)<br>
## Testing the rule
To be 100% sure that your rule will produce an E.164 formatted phone number, do test the rule to see it actually works as expected.<br>
![test-nr](https://github.com/danielurra/ms-teams-normalization-rules/assets/51704179/6f5b153c-a317-4361-98e0-5153241cfefb)


