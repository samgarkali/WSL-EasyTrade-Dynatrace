# WSL-EasyTrade-Dynatrace
Instruction on how to install WSL -> K3s -> Dynatrace Operator -> EasyTrade

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


### To run k3s commands without sudo
1. Change the permissions of the K3s configuration file
  ```
  sudo chmod 644 /etc/rancher/k3s/k3s.yaml
  ```
2. Set the `KUBECONFIG` environment variable
  ```
  export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
  ```


### (Optional) to add shortkey/alias
1. Open the `bashrc` file
  ```
  nano ~/.bashrc
  ```
2. Add your alias
  ```
  alias alias_name='command_to_run'
  ```
3. Save and exit
4. Apply the changes
  ```
  source ~/.bashrc
  ```



## Install Dynatrace Operator

Follow the instructions from here: https://docs.dynatrace.com/docs/shortlink/installation-k8s-cloud-native-fs#manifest



## Install EasyTrade on Kubernetes

Follow the instruction from here: https://github.com/Dynatrace/easytrade/

**Note:** Make sure to check following issue with the `rabbitmq.yaml`: https://github.com/Dynatrace/easytrade/issues/23


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
  kubectl create namespace easytrade
  ```
4. Apply manifest files
  ```
  kubectl -n easytrade apply -f easytrade/kubernetes-manifests
  ```
5. (Optional) To auto create problem patterns once a day
  ```
  kubectl -n easytrade apply -f easytrade/kubernetes-manifests/problem-patterns
  ```


### Useful commands
1. This command forwards port 80 of the `frontendreverseproxy-easytrade` service to port 8080 on your localhost
  ```
  kubectl port-forward --address 0.0.0.0 service/frontendreverseproxy-easytrade 8080:80 -n easytrade
  ```
2. After running the port-forward command, you can access the service from your browser or any other client using `http://localhost:8080`
