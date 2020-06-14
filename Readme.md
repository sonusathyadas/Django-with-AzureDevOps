## CI/CD of Django application to Azure App Servcie on Linux using Azure DevOps

In this tutorial, we are trying to deploy out Django web application into Azure App Service on Linux using the Azure DevOps pipeline. For completing this tutorial you need the following prerequisites:
    *) Azure Subscription
    *) Azure DevOps Account
    *) Azure CLI 2.x
    *) Python 3.5+ 
    *) Visual Studio Code
    *) Git 

### Create and Configure Azure App Service for Python deployment
1) Open the command prompt and run the following command to login to Azure.
    ```
    az login
    ```
2) Create a new resource group for this demo
    ```
    az group create --name AzureDevOpsGroup --location westus
    ```
3) Run the following command to create the 
    ```
    az appservice plan create -g AzureDevOpsGroup -l westus --name DevopsAppPlan --sku S1 --is-linux
    ```
4) Create a new app service web app using the following command. Replace `<your-app-name>` with your unique web application name.
    ```
    az webapp create -g AzureDevOpsGroup -l westus --name <your-app-name> --plan DevopsAppPlan --runtime "python|3.6"    
    ```
5) Optionally, you can update the version of the python runtime. Run the following command to update the python runtime version.
    ```
    az webapp config set -g AzureDevOpsGroup --name <your-app-name> --linux-fx-version "PYTHON|3.7"
    ```
6) Verify the runtime version by executing the following command.
    ```
    az webapp config show -g AzureDevopsGroup --name <your-app-name> --query linuxFxVersion
    ```
### Prepare the Django web application
1) We will be using a pre-created Django application for this tutorial. Open the command prompt and clone the github repository.
    ```
    git clone https://github.com/sonusathyadas/Django-with-AzureDevOps.git
    ```
2) Open the project folder in VS Code.
3) Open Integrated Terminal in VS Code and run the following command to create a virtual environment.
    ```
    python -m venv env
    ```
4) After the virtual environment is created, close the terminal. Press CTRL+SHIFT+P and choose the `Python: Select Interpreter` from the commands list. From the poped list choose the virtual environment you have created in the previous step.
5) Open the Integrated terminal and run the following command to install the project dependecies.
    ```
    pip install -r requirements.txt
    ```
6) Run the application and test the application in browser. 
    ```
    python manage.py runserver
    ```
7) You are now going to push the application to the Azure DevOps repository. The current project is already configured with the github repository. You need to remove the remote repository for the current project. Later we will add Azure DevOps Repos as the remote repository. Run the following command to remove the remote repository. Open terminal and run the command from the project root.
    ```
    git remote remove origin
    ```
8) You can verify whether the remote is removed or not using the following command. This should give an empty result.
    ```
    git remote -v
    ```
### Create and configure Azure DevOps project