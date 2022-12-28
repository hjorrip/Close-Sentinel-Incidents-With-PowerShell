# Close-Sentinel-Incidents-With-PowerShell

$rg = "<resource-group>" # UPDATE!
$wsn = "<workspace-name>" # UPDATE!! 

# Gets a list of Incidents to update and stores them in the $res variable.
$res = Get-AzSentinelIncident -ResourceGroupName $rg -WorkspaceName $wsn | Where-Object { $_.Title -like "Test*" -and $_.Status -ne 'Closed' } # UPDATE FILTER! TEST BEFORE EXECUTING NEXT LINE!

# TEST: Execute this line to see which Incidents will be effected
foreach ($object in $res) { Write-Output "Provider:$($object.Provider) - ProviderIncidentId:$($object.ProviderIncidentId) - Title:$($object.Title) - Severity:$($object.Severity) - Status:$($object.Status) - Classification(if closed):$($object.Classification)" }

# COMMIT: This will close all selected Incidents as 'Undetermined'
foreach ($object in $res) {
        Update-AzSentinelIncident -ResourceGroupName $rg -WorkspaceName $wsn -Id $object.Name -Title $object.Title -Severity $object.Severity -Classification 'Undetermined' -Status 'Closed' -Whatif
}
