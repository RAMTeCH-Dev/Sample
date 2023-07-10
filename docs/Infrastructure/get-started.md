## Get Started

The common workflow to get started with Chef Cookbooks for ArcGIS was to use the Cinc Client in local mode.  
This workflow included
- Installing Cinc Client on Windows Server
- Downloading Chef Cookbooks for ArcGIS in the appropriate locations
- Placing the required ArcGIS Setup and supporting setup files in appropriate locations
- Running Cinc Client in Local Mode
- Cleaning Up


#### Install Cinc Client

[CINC](https://cinc.sh/) is a community distribution of the open source software of Chef Software Inc. 
Cinc Client is a runtime for Chef cookbooks similar to Chef Infra Client. 
Chef Cookbooks for ArcGIS support Cinc Client as well as Chef Infra Client.

Cinc Client and all the required Chef cookbooks must be installed on each machine of the deployment.

See [cookbooks version compatibility](https://esri.github.io/arcgis-cookbook/versions.html) 
page for the Cinc Client version recommended for specific ArcGIS software versions.

##### Windows Setup
 
To install the latest 16.x of Cinc Client, open a Windows PowerShell terminal as administrator and run:

```
> . { iwr -useb https://omnitruck.cinc.sh/install.ps1 } | iex; install -version 16
```
<br/>


#### Download Chef Cookbooks and Deployment Templates

Download [arcgis-4.1.0-cookbooks.zip](https://github.com/Esri/arcgis-cookbook/releases/tag/v4.1.0) 
from Chef Cookbooks for ArcGIS [releases](https://github.com/Esri/arcgis-cookbook/releases) on GitHub 
to the machine and extract the contents of the archive to C:\cinc.

The archive contains ArcGIS-specific [Chef cookbooks](https://esri.github.io/arcgis-cookbook/cookbooks.html), 
required third-party cookbooks, 
and [deployment templates](https://esri.github.io/arcgis-cookbook/templates.html) for various ArcGIS configurations. 
The deployment templates located in �templates� directory inside the archive provide template JSON files 
for different machine roles, ArcGIS software versions, and platforms.

#### Edit Role JSON Files

Select a deployment template for the required ArcGIS subsystem, software version, and platform (Windows/Linux).

All the role JSON files in the deployment templates contain a run_list array 
that defines a list of recipes to run and attributes used by these recipes. 
The recipes are specified as <cookbook name>::<recipe name>.

Follow the instructions for the specific deployment template and the machine role for the required changes 
of attribute values.

#### Run Cinc Client in Local Mode

To execute recipes specified in the run_list in a role JSON file, 
run cinc-client on the machine as administrator.


```
> cinc-client -z -j <machine role JSON file path> --config-option cookbook_path=C:\cinc\cookbooks
```

#### Clean Up

Once all the Chef runs successfully complete, 
save the JSON files in a safe and secure place to use in the future for upgrades or disaster recovery.

The cinc-client reads attributes from the JSON files and caches the attributes inside a �nodes� folder 
that resides in the Chef workspace directory. Because some of the attributes in the JSON file may contain sensitive 
information, such as passwords, 
it is recommended that you delete the �nodes� folder in the Chef workspace directory after you complete the last Chef run on the machine.

Chef Cookbooks for ArcGIS extract the setup archives into the local directory specified by arcgis.repository.setups attribute. 
To save disk space, the archives and setups can be deleted after the machine configuration is completed.