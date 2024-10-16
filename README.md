# WSL-EasyTrade-Dynatrace
Instruction on how to install WSL -> Micro k8 -> EasyTrade -> Dynatrace Operator

----
## Install WSL on Windows OS

To install WSL follow this documentation: https://learn.microsoft.com/en-us/windows/wsl/install

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

## Install MicroK8s on Linux

Follow this instructions here: https://ubuntu.com/kubernetes/install

### Useful commands

* To access Kubernetes Dashboard
  ```
  microk8s dashboard-proxy
  ```
* Start and stop Kubernetes to save battery
  ```
  microk8s start/stop
  ```

### To create alias to replace `microk8s kubectl` with shortkey (`k` in this example)
1. Open the terminal
2. Open the `.bashrc` file
  ```
  nano ~/.bashrc
  ```
3. Add the alias to the end of the file
  ```
  alias k='microk8s kubectl'
  ```
4. Save and exit
    - `CTRL + S` - to save
    - `CTRL + X` - to exit
5. Apply the changes
  ```
  source ~/.bashrc
  ```
