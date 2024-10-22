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

## Install MiniKube on Linux

Follow this instructions here: [https://ubuntu.com/kubernetes/install](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download)


### Useful commands
```
minikube dashboard

minikube pause/unpause/stop
```


### To create alias to replace `minikube kubectl` with shorter version (`k` in this example)
1. Open the terminal
2. Open the `.bashrc` file
  ```
  nano ~/.bashrc
  ```
3. Add the alias to the end of the file
  ```
  alias k='minikube kubectl --'
  ```
4. Save and exit
    - `CTRL + S` - to save
    - `CTRL + X` - to exit
5. Apply the changes
  ```
  source ~/.bashrc
  ```

## Install EasyTrade on Kubernetes

Follow the instruction from here: https://github.com/Dynatrace/easytrade/

### Short summary on how to install
1. Install `git`
  ```
  sudo apt-get update
  sudo apt-get install git
  git --version
  ```
3. Clone the GitHub repo
  ```
  git clone https://github.com/Dynatrace/easytrade.git
  ```
3. Create namespace
  ```
  k create namespace easytrade
  ```
4. Apply manifest files
  ```
  k -n easytrade apply -f easytrade/kubernetes-manifests

  # Optional: to auto create problem patterns once a day
  k -n easytrade apply -f easytrade/kubernetes-manifests/problem-patterns
  ```


### Useful commands
* To access the easytrade frontend
```
minikube service frontendreverseproxy-easytrade -n easytrade
```
