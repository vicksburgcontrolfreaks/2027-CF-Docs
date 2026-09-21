Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
if (-not (Test-Path $PROFILE)) { New-Item -ItemType File -Path $PROFILE -Force | Out-Null }
Copy-Item -Path $PROFILE -Destination "$PROFILE.bak" -Force
Set-Content -Path $PROFILE -Value 'function gp { param([string]$m); cd "C:\Users\CFLT5\2027-CF-Docs"; git add .; git commit -m $m; git push }'
. $PROFILE

$gplFunction = @'
function gpl {
    cd "C:\Users\vicks\2027 CF"
    $originalBranch = git rev-parse --abbrev-ref HEAD
    $status = git status --porcelain
    if ($status) {
        $backupBranch = "backup-" + (Get-Date -Format "yyyy-MM-dd-HHmm")
        git checkout -b $backupBranch
        git add .
        git commit -m "Auto-backup before pull"
        git checkout $originalBranch
        Write-Host "Heads up: your local changes were different, so they were saved to a new branch: $backupBranch" -ForegroundColor Yellow
    }
    git pull
    Write-Host "Pull complete." -ForegroundColor Green
}
'@
Add-Content -Path $PROFILE -Value ("`n" + $gplFunction)
. $PROFILE