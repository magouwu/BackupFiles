$source = "C:\put\here\the\source"
$destination = "e:\put\here\the\destination"

#Put the year, first month, and last month (march = 3)
Get-ChildItem -Recurse $source | Where-Object {
    $_.LastWriteTime.Year -eq 2024 -and
    $_.LastWriteTime.Month -ge 1 -and
    $_.LastWriteTime.Month -le 8 -and
    -not $_.PSIsContainer
} | ForEach-Object {
    $relativePath = $_.FullName.Substring($source.Length)
    $destFile = Join-Path $destination $relativePath
     if (Test-Path $destFile) {
        # Comparar contenido
        $hashSource = Get-FileHash $_.FullName
        $hashDest = Get-FileHash $destFile
         if ($hashSource.Hash -eq $hashDest.Hash) {
            Write-Host "Ya existe (idéntico): $destFile"
            Remove-Item $_.FullName -Force
        } else {
            Write-Host "Conflicto (mismo nombre, diferente contenido): $destFile"
        }
    } else {
        # No existe → moverlo al destino
        $targetDir = Split-Path $destFile
        New-Item -ItemType Directory -Path $targetDir -Force | Out-Null
        Move-Item $_.FullName -Destination $destFile
        Write-Host "Movido a backup: $destFile"
    }
}
