# Lab Environment Setup

**[Home](../README.md)** - [Next Module >](../modules/aks-deployment.md)

## Prerequisites

* An [Azure account](https://azure.microsoft.com/free/) with an active subscription.
* Owner permissions within a Resource Group to create resources and manage role assignments.

## Deploy Jumpbox

1. Right-click or `Ctrl + click` the button below to open the Azure Portal in a new window.

    [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FBobbyH49%2Fazurearcsqlmanagedinstance%2Fmain%2Ftemplates%2Fjumpbox.json)

Deployment diagram (now also includes a Log Analytics workspace for monitoring your directly connected Managed Instances)

![deploy-diagram](media/deploy-diagram.png)

Deployed resources

![lab-resources](media/lab-resources-1.png)


## Lab Environment Setup - Automated

### Powershell scripts here

## Lab Environment Setup - Manual

### 1.  Install Client Tools on Jumpbox

**To install the tools you will need to open Powershell as Administrator and**

1.  Azure CLI

    For latest version go to https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli

    ```text
    $ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
    ```

    **Restart Powershell**

2.  ARCData extension for Azure CLI

    To install run 
    ```text
    az extension add --name arcdata
    ```
    ![arc-data-extension-install](media/arc-data-extension-install.png)

    To upgrade run 
    ```text
    az extension update --name arcdata
    ```
    ![arc-data-extension-update](media/arc-data-extension-update.png)

3.  Kubectl
    
    For latest version go to https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
    Example download using curl for version 1.24.0 

    ```text
    $ProgressPreference = 'SilentlyContinue'; mkdir C:\Kube; Invoke-WebRequest -Uri https://dl.k8s.io/release/v1.24.0/bin/windows/amd64/kubectl.exe -OutFile "C:\kube\kubectl.exe"
    ```
    ![kubectl-install](media/kubectl-install.png)


    Validate by running the following and comparing the two versions in SHA256 format

    ```text
    $ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri https://dl.k8s.io/v1.24.0/bin/windows/amd64/kubectl.exe.sha256 -OutFile "C:\kube\kubectl.exe.sha256"
    CertUtil -hashfile C:\kube\kubectl.exe SHA256
    type C:\kube\kubectl.exe.sha256
    ```
    ![kubectl-sha5](media/kubectl-sha5.png)

    Ensure you are able to run Kubectl in **Powershell**

    ```text
    $env:Path += "C:\kube;"
    [Environment]::SetEnvironmentVariable("Path", $env:Path, [System.EnvironmentVariableTarget]::Machine)
    ```
    ![kubectl-path](media/kubectl-path.png)

    **Restart Powershell**

    ```text
    kubectl version --client
    ```
    ![kubectl-version](media/kubectl-version.png)


### 2. [Deploy Azure AKS Cluster](./aks-deployment.md)
### 3. [Deploy Azure Arc Enabled SQL MI with Indirect Connectivity](./indirect.md)
### 4. [Deploy Azure Arc Enabled SQL MI with Direct Connectivity](./direct.md)
