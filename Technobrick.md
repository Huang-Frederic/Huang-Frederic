
# 📍 Embedded AI Lab Dash Template

The Embedded AI Lab Dash Template is a comprehensive framework designed to simplify the development of Dash-based web applications, integrating seamlessly with Python, Azure, and Dataiku environments. This template accelerates the application creation process by providing a pre-configured structure, essential components, and detailed guidance for deployment across diverse platforms. Its compatibility with Azure ensures smooth cloud-based hosting and scaling, making it an ideal solution for developers aiming to leverage the cloud for their applications. The template caters to developers seeking a streamlined approach to deploying interactive data visualizations and AI-driven dashboards, whether working locally, in enterprise ecosystems like Dataiku, or in cloud environments like Azure.


## 🔮 Stack

![Static Badge](https://img.shields.io/badge/python-gray?style=for-the-badge&logo=Python)
![Static Badge](https://img.shields.io/badge/dash-gray?style=for-the-badge&logo=Dash)
![Static Badge](https://img.shields.io/badge/plotly-gray?style=for-the-badge&logo=Plotly)
![Static Badge](https://img.shields.io/badge/dataiku-gray?style=for-the-badge&logo=Dataiku)

> [!NOTE]  
> This project has been developed and documented on DSS13, dash 2.18.0 and AI_LAB_DC 0.1.13.


## 🌐 Project Overview

### File Structure

The core files and directories are organized as follows:

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

- Python 3.8 or higher
- Package managers : pip

## 🛰️ Environnement Variables

This template does not use a .env file. However, you must manually specify the assets_folder location in the main script.

### Default 

By default, Dash manages most configurations automatically, and you can define the assets_folder as follows:

```python
app = Dash(
    __name__,
    external_stylesheets=[dbc.themes.BOOTSTRAP],
    assets_folder="AI_Lab_DC/assets",
)
```

### Dataiku

In Dataiku, defining the assets_folder requires extra steps due to restricted library folder access. To work around this, you need to upload the assets folder to the Webapp resources and use a helper function to adjust paths.

Steps:

1 - Upload the assets folder:

- Navigate to your project, open the </> menu, and click Libraries (shortcut: G+L).
- In the Resources section (top right), locate or create a folder named lib/local-static.
- Upload the entire assets folder of this repo in the local-static folder.

2 - Configure the main script:
Modify main.py to adjust the paths dynamically:

```python
from dataiku import default_project_key
from AI_Lab_DC import initAssetsDir

# Setup assets
assets_dir = f"/local/projects/{default_project_key()}/public-resources/assets"
initAssetsDir(assets_dir)
app.config.external_stylesheets = [dbc.themes.BOOTSTRAP, f"{assets_dir}/style.css"]
```

> [!NOTE]  
> The default_project_key() function retrieves the current project key, while initAssetsDir updates all paths in the library to use the specified assets_folder.
> initAssetsDir is a function that refractor the path to the assets folder for the whole lib.

## 💻 Installation

### From source

> [!TIP]  
> It's recommended to create a virtual environment for this project to keep dependencies isolated.

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
> Ensure you have an Azure personal access token with permissions to read packages. You can create one with "Read" rights under the Packaging section.

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

You can extend the project by adding custom components in the components folder. Simply create a function that returns your desired component and include it in the __init__.py file. Similarly, new callbacks can be added to the callbacks folder, following the structure of existing examples. For assets like styles or images, place them in the appropriate subfolder within assets.

Artifacts are managed by the pipeline, so any changes pushed to the main branch will automatically update the project. If you're making significant updates, remember to modify the version in the pyproject.toml file to reflect the changes.
