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
  wsl --install -d Ubuntu 
  ```

### To disable WSL password
1. In wsl add **your username** to a sudo config file
  ```
  sudo nano /etc/sudoers.d/MY_USERNAME
  ```
2. Add the following line
  ```
  MY_USERNAME ALL=(ALL) NOPASSWD:ALL
  ```

## Install K3s

Follow the instructions from here: https://docs.k3s.io/quick-start

### Steps to follow
1. Run the installation script
```
curl -sfL https://get.k3s.io | sh -
```
2. Verrify the installation
```
sudo k3s kubectl get nodes
```

### To run k3s command without sudo
1. Change the permissions of the K3s configuration file
```
sudo chmod 644 /etc/rancher/k3s/k3s.yaml
```
2. Set the `KUBECONFIG` environment variable
```
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
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
