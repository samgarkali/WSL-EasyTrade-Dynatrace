# WSL-EasyTrade-Dynatrace
Instruction on how to install WSL -> K3s -> Dynatrace Operator -> EasyTrade

## Paragrpahs
1. [Install WSL on Windows OS](#install-wsl-on-windows-os)
2. [Install K3s](#install-k3s)
3. [Install Dynatrace Operator](#install-dynatrace-operator)
4. [Install EasyTrade](#install-easytrade)
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


### Useful commands
1. Copy file from WSL to Windows
  ```
  cp /home/john/wsl-file.txt /mnt/c/Users/your_username/Downloads  
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


### (Optional) To add shortkey/alias
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


### To start `k3s` automatically when running WSL
1. Create a startup script: Create a script file, for example, start-k3s.sh, in your home directory:
  ```
  nano ~/start-k3s.sh
  ```
2. Add the command to start k3s: Add the following line to the script:
  ```
  sudo systemctl start k3s
  ```
3. Make the script executable:
  ```
  chmod +x ~/start-k3s.sh
  ```
4. Configure WSL to run the script on startup: Edit the WSL configuration file /etc/wsl.conf (create it if it doesn’t exist):
  ```
  sudo nano /etc/wsl.conf
  ```
5. Add the following lines to the file:
  ```
  [boot]
  command="sh /home/your-username/start-k3s.sh"
  ```
6. Restart WSL: Close your WSL terminal and run the following command in **Command Prompt** or **PowerShell**
  ```
  wsl --shutdown
  ```
7. Open the WSL. The k3s service should start automatically.



## Install Dynatrace Operator

Follow the instructions from here: https://docs.dynatrace.com/docs/shortlink/installation-k8s-cloud-native-fs#manifest


### Steps to follow
1. Create a dynatrace namespace
  ```
  kubectl create namespace dynatrace
  ```
2. Install Dynatrace Operator
  ```
  kubectl apply -f https://github.com/Dynatrace/dynatrace-operator/releases/download/v1.3.2/kubernetes.yaml
  kubectl apply -f https://github.com/Dynatrace/dynatrace-operator/releases/download/v1.3.2/kubernetes-csi.yaml
  ```
3. Run the following command to see when Dynatrace Operator components finish initialization:
  ```
  kubectl -n dynatrace wait pod --for=condition=ready --selector=app.kubernetes.io/name=dynatrace-operator,app.kubernetes.io/component=webhook --timeout=300s
  ```
4. Create secret for Access tokens named dynakube for the Dynatrace Operator token and data ingest token obtained in Tokens and permissions required.
  ```
  kubectl -n dynatrace create secret generic dynakube --from-literal="apiToken=<OPERATOR_TOKEN>" --from-literal="dataIngestToken=<DATA_INGEST_TOKEN>"
  ```
5. Apply the DynaKube custom resource
  ```
  kubectl apply -f <your-DynaKube-CR>.yaml
  ```
6. **(Optional)** Verify that your DynaKube is running and all pods in your Dynatrace namespace are running and ready.
  ```
  kubectl get dynakube -n dynatrace
  kubectl get pods -n dynatrace
  ```



## Install EasyTrade

Follow the instruction from here: https://github.com/Dynatrace/easytrade/

**Note:** Make sure to check following issue with the `rabbitmq.yaml`: https://github.com/Dynatrace/easytrade/issues/23


### Steps to follow
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
5. **(Optional)** To auto create problem patterns once a day
  ```
  kubectl -n easytrade apply -f easytrade/kubernetes-manifests/problem-patterns
  ```


### Useful commands
1. This command forwards port 80 of the `frontendreverseproxy-easytrade` service to port 8080 on your localhost
  ```
  kubectl port-forward --address 0.0.0.0 service/frontendreverseproxy-easytrade 8080:80 -n easytrade
  ```
2. After running the port-forward command, you can access the service from your browser or any other client using [http://localhost:8080](http://localhost:8080)
