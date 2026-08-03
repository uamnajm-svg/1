name: Build Windows RDP with Tailscale

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest
    steps:
      - name: Enable Remote Desktop
        run: |
          Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0
          Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

      - name: Set RDP Password
        run: |
          $admin = [adsi]"WinNT://./runneradmin,user"
          $admin.SetPassword("P@ssword123456")

      - name: Install Tailscale
        run: |
          Invoke-WebRequest -Uri "https://pkgs.tailscale.com/stable/tailscale-setup-1.82.0-amd64.msi" -OutFile "$env:TEMP\tailscale.msi"
          cmd /c msiexec.exe /i "%TEMP%\tailscale.msi" /quiet /qn /norestart
          & "C:\Program Files\Tailscale\tailscale.exe" up --authkey ${{ secrets.TAILSCALE_AUTHKEY }} --unattended --accept-routes

      - name: Keep Alive RDP
        run: |
          while ($true) { Start-Sleep -Seconds 3600 }
