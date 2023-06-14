# Normalization Rules (MS Teams)
This small and simple script was used to bulk provision normalization rule for MS Teams.
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
