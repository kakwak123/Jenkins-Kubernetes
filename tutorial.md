## Setting Up Windows Subsystem for Linux (WSL)

### Getting Started with WSL
Windows Subsystem for Linux is now available in the Microsoft Store! Installing WSL from the Microsoft Store ensures you receive the latest updates faster.

- **Upgrade WSL**: Run the command:
  ```bash
  wsl.exe --update
  ```
  Alternatively, you can visit the [Microsoft Store WSL Page](https://aka.ms/wslstorepage).

- **More Information**: For additional details, visit the [WSL Store Info Page](https://aka.ms/wslstoreinfo).

### Welcome to Ubuntu 20.04 LTS
Upon installation, you'll be running GNU/Linux 5.10.16.3-microsoft-standard-WSL2 x86_64. Here’s some useful information:
- **Documentation**: [Ubuntu Documentation](https://help.ubuntu.com)
- **Management**: [Canonical Landscape](https://landscape.canonical.com)
- **Support**: [Ubuntu Advantage](https://ubuntu.com/advantage)

### System Information Snapshot (as of Mon Jun 24 20:23:27 AEST 2024)
- System Load: 0.08
- Memory Usage: 4%
- Disk Usage: 0.9% of 250.98GB
- IPv4 Address for eth0: 172.20.81.43

### Daily Message Management
To stop the daily system information message:
```bash
touch /home/kakwak/.hushlogin
```

## Installing and Configuring Jenkins on Kubernetes via WSL

### Step 1: Install Helm
Run the following command to download and install Helm, a package manager for Kubernetes:
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

### Step 2: Set Up Jenkins Repository
Initialize the Jenkins Helm chart repository:
```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update
```

### Step 3: Deploy Jenkins
Deploy Jenkins using Helm:
```bash
helm install my-jenkins jenkins/jenkins
```
Note: If you encounter a "Kubernetes cluster unreachable" error, ensure that Kubernetes is running and that `kubectl` is correctly configured.

### Step 4: Retrieve Jenkins Admin Password
To get the Jenkins 'admin' user password:
```bash
kubectl exec --namespace default -it svc/my-jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo
```

### Step 5: Access Jenkins Dashboard
Run the following commands in the same terminal to forward ports and access Jenkins:
```bash
echo http://127.0.0.1:8080
kubectl --namespace default port-forward svc/my-jenkins 8080:8080
```

### Step 6: Log in to Jenkins
Use the retrieved password and the username `admin` to log in at `http://127.0.0.1:8080`.

### Step 7: Configure Jenkins
- **Security**: Set up the security realm and authorization strategy according to your organization's requirements.
- **Configuration as Code**: Utilize Jenkins Configuration as Code by specifying `configScripts` in your `values.yaml` file. For documentation and examples:
  - [Jenkins Configuration as Code Documentation](http://127.0.0.1:8080/configuration-as-code)
  - [Configuration as Code Examples on GitHub](https://github.com/jenkinsci/configuration-as-code-plugin/tree/master/demos)

For comprehensive guidance on running Jenkins on Kubernetes, visit [Google Cloud Solutions for Jenkins](https://cloud.google.com/solutions/jenkins-on-container-engine).

For more details on Jenkins Configuration as Code, see [Jenkins Configuration as Code Project](https://jenkins.io/projects/jcasc/).
