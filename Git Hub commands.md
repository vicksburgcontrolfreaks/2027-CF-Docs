Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
if (-not (Test-Path $PROFILE)) { New-Item -ItemType File -Path $PROFILE -Force | Out-Null }
Copy-Item -Path $PROFILE -Destination "$PROFILE.bak" -Force
Set-Content -Path $PROFILE -Value 'function gp { param([string]$m); cd "C:\Users\CFLT5\2027-CF-Docs"; git add .; git commit -m $m; git push }'
. $PROFILE