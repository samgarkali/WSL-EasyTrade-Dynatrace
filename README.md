# WSL-EasyTrade-Dynatrace
Instruction on how to install WSL -> Micro k8 -> EasyTrade -> Dynatrace Operator

----
## Install WSL on Windows OS

1. https://learn.microsoft.com/en-us/windows/wsl/install

### To reset WSL configurations

Option A:
1. Windows Settings > Apps > Installed apps
2. Find "Ubuntu" > Advanced options
3. Click on "Reset"

Option B:
1. Search for `cmd` and run as an admin
2. List installed Distros
  ```
    wsl --list
  ```
3. Unregister Ubuntu:
  ```
    wsl --unregister Ubuntu
  ```
4. Reinstall Ubuntu:
  ```
    wsl -install -d Ubuntu 
  ```
