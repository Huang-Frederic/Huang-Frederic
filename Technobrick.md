
# 📍 Embedded AI Lab Dash Template

This is a Template that you can clone to start your Dash project, it will contain all the components needed for a fast and easy release.


## 🔮 Stack

![Static Badge](https://img.shields.io/badge/python-gray?style=for-the-badge&logo=Python)
![Static Badge](https://img.shields.io/badge/dash-gray?style=for-the-badge&logo=Dash)
![Static Badge](https://img.shields.io/badge/plotly-gray?style=for-the-badge&logo=Plotly)
![Static Badge](https://img.shields.io/badge/dataiku-gray?style=for-the-badge&logo=Dataiku)

> [!NOTE]  
> This project has been developed and documented on DSS13, dash 2.18.0 and AI_LAB_DC 0.1.13.


## 🌐 Project Overview

### File Structure

The key files and directories related are structured as follows:

```txt
AI_Lab_DC/
├── assets/
│   └── fonts/
│   └── style.css
│   └── *.svg / *.png / *.ico
├── callbacks/
├── components/
├── main.py
```

## 🚀 Getting Started

System Requirements:

- Python 3.8+
- Package managers : pip

## 🛰️ Environnement Variables

There is no .env file but in the main, you will have to define the assets_folder. 

### Default 

By default you can setup the location of the assets folder, the rest is completly handled by Dash.

```python
app = Dash(
    __name__,
    external_stylesheets=[dbc.themes.BOOTSTRAP],
    assets_folder="AI_Lab_DC/assets",
)
```

### Dataiku

On Dataiku, it's a little tricky, the problem is that you cannot define the assets folder since you cannot access to folders of libraries, so in this library you have a global variable the library use to refractor all the paths that use assets. But beforehand, you will need to upload the assets folder in the resources of your Dataiku Webapp :

- From your project home, hover the "</>" section of the menu and click on Libraries (or type G+L).
- On the top right you have a section called resources you should see a folder called lib/local-static, if not, create the folder.
- Upload the entire assets folder of this repo in the local-static folder.
- Go on your main.py, and rather that define the assets_folder during the creation of the app such as above, define it as following.

```python
# Get the key to access public resources
from dataiku import default_project_key

# Import the sys module for the assets
from AI_Lab_DC import initAssetsDir

# Setup assets
assets_dir = f"/local/projects/{default_project_key()}/public-resources/assets"
initAssetsDir(assets_dir)
app.config.external_stylesheets = [dbc.themes.BOOTSTRAP, f"{assets_dir}/style.css"]
```

> [!NOTE]  
> To get the path to the resources, you need an key that you can retrieve using the function default_project_key.
> initAssetsDir is a function that refractor the path to the assets folder for the whole lib.

## 💻 Installation

### From source

> [!TIP]  
> It's always better to create an env for your different project, and it should be the case in this one.

```bash
$ pip install venv
# if not installed yet
$ python -m venv env
$ env\Scripts\activate 
```
Now you should be in the environnement, and everything you install in this state will be wall off within the project.

```bash
$ pip install AI-Lab-DC --extra-index-url https://{Personnal_Access_Token}@pkgs.dev.azure.com/slb-swt/AI_Lab/_packaging/AI_Lab_Template/pypi/simple
# The download should take some time.
```
> [!WARNING]  
> You need to create an access token in azure, with the right to read packages (Check Read in the Packaging section when create an access token).

Now you can use the library ! To get start, you can use the main.py available on this repo, it's a template you can use as default.

```bash
$ python main.py
# to run your app
```
### From Dataiku

On Dataiku, you will also need an Environnement, to do so, go to Administration > Code Envs (on the top right). You need to request a Python env if you don't have one. 
Once you selected your env, go to General > Extra options section, and add an Extra options for "pip install", and add the extra-index-url as an option for "PIP INSTALL".

```bash
--extra-index-url=https://{Personnal_Access_Token}@pkgs.dev.azure.com/slb-swt/AI_Lab/_packaging/AI_Lab_Template/pypi/simple
```

After that, go to Packages to install and simply add the package, save and update. 

```bash
AI-Lab-DC
```

> [!TIP]  
> After the update you can check if the package has been installed successfully in the currently installed packages, make sure to check on which version you start to develop.

The package is now installed on your environnement, now we have to create the webapp : 
- From your project home, hover the "</>" section of the menu and go to webapps.
- Click New Webapp > Code Webapp > Dash.
- In the Settings of this app, make sure the right code env is selected.

You can now start to develop ! you can use the main.py available on this repo, it's a template you can use as default.

> [!WARNING]  
> If you haven't defined the assets folder, please make sure to go above at "🛰️ Environnement Variables" section. Otherwise, the images and css won't be applied for the components. 

## 🛠️ Components

## 🚶‍♂️ Author

- [@Huang-Frederic](https://github.com/Huang-Frederic)

## 🔗 How to contribute ?

- Local
- Artifact already done

## 🔗 Acknowledgements

Red River West Recruitment :

- [Technical Test Instructions](https://redriverwest.notion.site/Fullstack-Developer-Internship-Take-home-technical-test-059113beb6d549e49baffc50b7500bdd)
- [Imposed Design](https://www.figma.com/design/ZpGmtyYlHS344OI8J6cD79/Untitled?node-id=0-1)

Usefull Documentations :

- [Nuxt.JS](https://nuxt.com/?uwu=true)
- [Supabase](https://supabase.com/)
