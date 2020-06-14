# CI/CD of Django application to Azure App Servcie on Linux using Azure DevOps

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
1) Open Azure DevOps portal by navigating to (https://dev.azure.com)[https://dev.azure.com] and login to your Azure DevOps account.
2) Create a new DevOps project.
    ![Create project](/resources/image1.png)
3) Open the `Repos` and copy the git commands for uploading an existing git repository.
    ![upload repo](/resources/image2.png)
4) Copy the commands and execute in the VS Code integrated terminal. This will add the Azure repos as the remote repository and push the code to it.
    ![upload repo](/resources/image3.png)
5) You will see the uploaded files in the Azure Repos.
    ![upload repo](/resources/image4.png)
6) Click on the `Pipelines` in the Azure DevOps portal and click on `Create pipeline` to create a new pipeline.
    ![Create pipeline](/resources/image5.png)
7) Select the `Azure Repos Git` as the code source.
    ![Create pipeline](/resources/image6.png)
8) Select your DevOps project and the repository
    ![Create pipeline](/resources/image7.png)
9) Choose `Python to Linux Web App on Azure` as the deployment target. This will popup the available subscriptions. Select the subscription and click `Continue`. You may need to authenticate yourself to continue with that subscription.
    ![Create pipeline](/resources/image8.png)
10) Select the App service web app name from the dropdownlist. You can select the App Service web app name that you have created above. Click on `Validate and configure` to continue.
    ![Create pipeline](/resources/image9.png)
11) This will generate the YAML file with the build and deploy configuration code.
    ![Create pipeline](/resources/image10.png)
12) Click on the `Save and Run` button to save the YAML file in the repository folder and execute the pipeline.
    ![Create pipeline](/resources/image11.png)
13) Once the deployment is completed, you can navigate to the App Service web app and browse the site. You will be able to see the Django application running on the App Service.
    ![Create pipeline](/resources/image12.png)