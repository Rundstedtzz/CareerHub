# Vanderbilt Data Science Institute - DGX A100 Narrative Arc Guide

A guide to using the DGX for the Narrative Arc project.

# Quick navigation

[Quick Command Reference](#quick-command-reference)\
[Existing Resources](#existing-resources)\
[Connecting to the DGX](#connecting-to-the-dgx)\
[Creating a new Docker Volume](#creating-a-new-docker-volume)\
[Use a Github repository with the docker container](#use-a-github-repository-with-the-docker-container)\
[Starting a new PyTorch docker container and launching Jupyter Lab](#starting-a-new-pytorch-docker-container-and-launching-jupyter-lab)\
[Modifying and saving a PyTorch docker container to include new libraries](#modifying-and-saving-a-pytorch-docker-container-to-include-new-libraries)\
[Using a shared docker volume and your personal docker volume at the same time](#using-a-shared-docker-volume-and-your-personal-docker-volume-at-the-same-time)\
[Using Google Drive and the DGX](#using-google-drive-and-the-dgx)

# Quick Command Reference
Connect to DGX:<br>
ssh -L {port}:localhost:{port} {username}@10.33.2.11<br><br>
Launch docker:<br>
docker run --gpus all --net=host -it -v {docker volume}:/workspace/{docker volume} -v narrative-arc:/workspace/narrative-arc na_docker


# Existing Resources

1. [DGX User Guide](https://github.com/vanderbilt-data-science/dgx-user-guide)
2. [Running Jupyter Notebooks on the DGX](https://github.com/vanderbilt-data-science/dgx-user-guide/blob/main/using-docker-jupyter.md)

First, please start with the resources above. Following the information in the resources above, you should have access to the DGX and be able to connect to it.

# Connecting to the DGX
For more detailed steps, please visit [Running Jupyter Notebooks on the DGX](using-docker-jupyter.md)

1. Request an account be created for you on the DGX. [DGX User Setup Form (Compute Grant)](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link)

2. Get your assigned port number for your personal use.

3. If you have not already, set up the Vanderbilt VPN on your machine. [Vanderbilt IT - Remote Access-SSL VPN](https://it.vanderbilt.edu/services/catalog/end-point_computing/network_access/remote-access/index.php)

4. Connect to the Vanderbilt VPN.

5. SSH to the DGX machine.<br>
Generic Example:<br>
ssh -L XXXX:localhost:XXXX username@DGX_IP_ADDRESS<br><br>
Real-world Example:<br>
ssh -L 1234:localhost:1234 doej@10.33.2.11

**PLEASE NOTE:** When connecting via SSH, you will need to use the keyfile that was generated and provided when getting a DGX account. [Adding your SSH key to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)

# Creating a new Docker Volume

**Deleting a docker volume is not easy - ensure you are ok with the name of the volume you created**

If a new docker volume is needed for a new docker container, please login to the DGX and then use the instructions below:


1. Check existing docker volume names so as not to use an existing name:<br>
docker volume ls

2. Create a directory for the volume to use in the /raid/username directory.<br>
Generic Example:<br>
cd /raid/username<br>
mkdir testfolder<br><br>
Real-world Example:<br>
cd /raid/doej<br>
mkdir n_arc

3. Create a new docker volume:<br>
Generic Example:<br>
docker volume create --driver local --opt type=none --opt device=/raid/username/testfolder --opt o=bind testfolder<br><br>
Real-world Example:<br>
docker volume create --driver local --opt type=none --opt device=/raid/doej/n_arc --opt o=bind n_arc

4. Check that the volume now exists:<br>
docker volume ls

# Use a Github repository with the docker container

In order to get the latest code from a repository onto the DGX, the following steps are required:
1. On the DGX, navigate to the directory used by the docker volume. Again, the `docker volume ls` command will list the available docker volumes.<br>
Generic Example:<br>
cd /raid/username/testfolder<br><br>
Real-world Example:<br>
cd /raid/doej/n_arc

2. Perform a `git clone` with the appropriate repository. (Ensure that you have a [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic) from your Github account.)<br>
Generic Example:<br>
git clone https://github.com/repository-name<br><br> 
Real-world Example:<br>
git clone https://github.com/vanderbilt-data-science/narrative-arc.git

Since the git clone command generates a new directory under the directory used by the docker volume, it will be accessible inside of the docker container at the location:<br>
Generic Example:<br>
/workspace/testfolder/repository_folder<br><br>
Real-world Example:<br>
/workspace/n_arc/narrative-arc

After making changes to the files, git commit and push commands can be used to update the Github repository.

# Starting a new PyTorch docker container and launching Jupyter Lab

1. Navigate to the [PyTorch container website](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch) and click the "Copy Image Path" button to select the most recent PyTorch container. This will add the path to the clipboard.

2. On the DGX, navigate to the directory used by the docker volume. Again, the `docker volume ls` command will list the available docker volumes.<br>
Generic Example:<br>
cd /raid/username/testfolder<br><br>
Real-world Example:<br>
cd /raid/doej/n_arc

3. Run the following command to create and launch the docker container:<br>
Generic Example:<br>
docker run --gpus all --net=host -it -v testfolder:/workspace/testfolder PYTORCH_IMAGE_PATH_COPIED_FROM_WEBSITE<br><br>
Real-world Example:<br>
docker run --gpus all --net=host -it -v n_arc:/workspace/n_arc nvcr.io/nvidia/pytorch:23.02-py3<br><br>

4. After running the above command, you will automatically be at the command prompt inside of the docker container at `/workspace/`. The directory you are in when launching Jyputer Lab, will be the top directory inside of Jupyter Lab. Therefore, if you have multiple docker volumes, it is important to launch Jupyter Lab from /workspace/ and not inside of a different directory.<br>Launch Jyputer Lab with the appropriate port number used when connecting to the DGX:<br>
Generic Example:<br>
jupyter-lab --port XXXX
Real-world Example:<br>
jupyter-lab --port 1234

5. When the Jyputer server starts, a url will be printed that can be copied to a local browser and modfiied to access the remote server:<br>
hostname -> localhost<br>
8888 -> port number used when connecting to the DGX<br>
printed url: http://hostname:8888/?token=985cbecb75aa4d0c86a002e1cfeb7e8d0e3bc339c38152b2<br>
url to put into a browser on the local machine: http://localhost:1234/?token=985cbecb75aa4d0c86a002e1cfeb7e8d0e3bc339c38152b2<br><br>

# Modifying and saving a PyTorch docker container to include new libraries
1. Launch the PyTorch docker container using the steps above.

2. Once in the docker container do a `pip install` of the missing libraries.<br>
**For the Narrative Arc project, the following pip command was run to install additional libraries on the base PyTorch container:**<br>
pip install datasets transformers huggingface-hub google-api-python-client google-auth-httplib2 google-auth-oauthlib ipywidgets


3. Make a second SSH connection into the DGX.

4. From the second connection, check running docker images by running `docker ps`. Note the CONTAINER_ID of the PyTorch container running in the other SSH connection.

5. Run the following docker command to create a new image:<br>
Generic Example:<br>
docker commit CONTAINER_ID NEW_IMAGE_NAME<br><br>
Real-world Example:<br>
docker commit d4576eda8284 na_docker

6. To run the newly created container run:<br>
Generic Example:<br>
docker run --gpus all --net=host -it -v testfolder:/workspace/testfolder na_docker<br><br>
Real-world Example:<br>
docker run --gpus all --net=host -it -v n_arc:/workspace/n_arc na_docker

# Using a shared docker volume and your personal docker volume at the same time
There are many instances where you will want to use your personal docker volume with your code/branch/repository, but you also want to be able to use a shared docker volume to read/write shared files. <br>

To do this you can launch a docker container with multiple volumes attached.<br>If you need to create a docker volume (either personal or shared) please see the instructions above.<br><br>

To run a docker container with multiple volumes use the following commmand:<br>
**docker run --gpus all --net=host -it -v {AAA}:/workspace/{BBB} -v {CCC}:/workspace/{DDD} {docker image name}**<br>
AAA = docker volume 1 name<br>
BBB = folder name used to access the docker volume 1 inside the running container<br>
CCC = docker volume 2 name<br>
DDD = folder name used to access the docker volume 2 inside the running container<br><br>

Genereic Example:<br>
docker run --gpus all --net=host -it -v testvolume1:/workspace/testvolume1 -v testvolume2:/workspace/testvolume2 na_docker<br><br>
Real-world Example:<br>
docker run --gpus all --net=host -it -v na-brian:/workspace/na-brian -v narrative-arc:/workspace/narrative-arc na_docker

# Using Google Drive and the DGX
To use Google Drive, you will need to create a service account to be used by the Google Drive APIs. This is critical because trying to use a normal Google Account with the Google Drive APIs requires a local webserver to be started to listen for responses from the Google servers. Since the DGX has ports locked down for security, this is not a viable solution. Therefore, a service account is required as that does not require a port to be opened to communicate with the Google Drive APIs.


**Setup Google Drive API environment and project**
1. Create a Google Cloud project with the following documentation: https://developers.google.com/workspace/guides/create-project
2. After creating the project, ensure the project name appears in the project dropdown at the top left of the page. Then from the menu on the left side of the page, select "APIs & Services" > "Enabled APIs & services".
3. Click "+ ENABLE APIS AND SERVICES" at the top of the page.
4. In the search bar, type "Google Drive" and select "google drive api".
5. From the results, click the first option which should be "Google Drive API"
6. Click the "ENABLE" button.

***Setup the service account and Google Drive API environment***
1. Go to: https://console.developers.google.com/iam-admin/serviceaccounts or from inside of the Google Cloud dashboard, choose the menu and select "IAM & Admin" > "Service Accounts"
2. Select your project from the list of "recent projects." 
3. From the "Service account for project "PROJECT_NAME" page, selecte "+ CREATE SERVICE ACCOUNT" from the top of the page, just under the search text field.
4. Give the account a name and id and click the "CREATE AND CONTINUE" button.
5. In the "Grant this service account access to project" select "Owner" from the dropdown and click "CONTINUE".
6. In the "Grant users access to this service account", type YOUR Google Account's email address into both fields and when your account appears, select that account. Finally, click "DONE".
7. When returned to the "Service accounts for project "PROJECT_NAME" page, click the new account that was just created. Select the "KEYS" option.
8. In the Keys section, click the "ADD KEY" dropdown and click "Create new key".
9. Leave the key type as JSON and click CREATE.
10. Save the downloaded json to be used later.

***Go to Google Drive share and add service account***
1. Go to the Google Drive share and click the icon for sharing the directory.
2. Paste in the service account name (ie. testdgx@sampledgx.iam.gserviceaccount.com) and set it to have the appropriate permissions.

***Connect to the Goolge APIs in a Jupyter Notebook***
1. With the Google Drive API environment setup, copy the key file (JSON) that was previously downloaded to the DGX.
2. 30-Google_Drive_DGX_integration.ipynb is a sample Jupyter notebook that connects to Google Drive, (the Google Drive folder used by Narrative Arc.)
3. Please note that 30-Google_Drive_DGX_integration.ipynb uses the key file named "jupyterlab.json". You will need to either change your file name or the code to read the appropriate file. Also, by default the notebook expects the key file to be in the same directory as the notebook file.

***Helpful links***
Creating a new Google Cloud project: https://developers.google.com/workspace/guides/create-project<br>
Creating a Google Cloud service account: https://developers.google.com/identity/protocols/oauth2/service-account<br>
Google Drive APIs Quickstart Guide Python: https://developers.google.com/drive/api/quickstart/python#set_up_your_environment<br>
Google Drive API Guide: https://developers.google.com/drive/api/guides/about-files
