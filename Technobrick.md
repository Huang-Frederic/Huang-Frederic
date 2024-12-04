
# 📍 Embedded AI Lab Dash Template

A comprehensive framework for building and deploying Dash-based AI-driven dashboards, seamlessly integrating with Python, Azure, and Dataiku for streamlined development and cloud scalability.

The **Embedded AI Lab Dash Template** is a comprehensive framework designed to simplify the development of **Dash-based web applications**, integrating seamlessly with **Python**, **Azure**, and **Dataiku** environments. This template accelerates the **application creation process** by providing a **pre-configured structure**, **essential components**, and detailed guidance for **deployment across diverse platforms**. Its compatibility with **Azure** ensures smooth **cloud-based hosting and scaling**, making it an ideal solution for developers aiming to leverage the **cloud** for their applications. The template caters to developers seeking a streamlined approach to deploying **interactive data visualizations** and **AI-driven dashboards**, whether working **locally**, in enterprise ecosystems like **Dataiku**, or in **cloud environments** like **Azure**.

## 🔮 Stack

![Static Badge](https://img.shields.io/badge/python-gray?style=for-the-badge&logo=Python)
![Static Badge](https://img.shields.io/badge/dash-gray?style=for-the-badge&logo=Dash)
![Static Badge](https://img.shields.io/badge/plotly-gray?style=for-the-badge&logo=Plotly)
![Static Badge](https://img.shields.io/badge/dataiku-gray?style=for-the-badge&logo=Dataiku)

> [!NOTE]  
> This project has been developed and documented on Dataiku `DSS13`, Dash `ver.2.18.0` and AI_LAB_DC `ver.0.1.13`.


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

- Python `3.8` or higher
- Package managers : `pip`

## 🛰️ Environnement Variables

This template does not use a `.env` file. However, you must manually specify the `assets/` folder location in the main script.

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

In Dataiku, defining the `assets/` folder requires extra steps due to restricted library folder access. To work around this, you need to upload the assets folder to the Webapp resources and use a helper function to adjust paths.

Steps:

1 - Upload the assets folder:

- Navigate to your project, open the `</>` menu, and click Libraries (shortcut: `G+L`).
- In the Resources section (top right), locate or create a folder named `lib/local-static`.
- Upload the entire assets folder of this repo in the local-static folder.

2 - Configure the main script:
Modify `main.py` to adjust the paths dynamically:

```python
from dataiku import default_project_key
from AI_Lab_DC import initAssetsDir

# Setup assets
assets_dir = f"/local/projects/{default_project_key()}/public-resources/assets"
initAssetsDir(assets_dir)
app.config.external_stylesheets = [dbc.themes.BOOTSTRAP, f"{assets_dir}/style.css"]
```

> [!NOTE]  
> The `default_project_key()` function retrieves the current project key, while initAssetsDir updates all paths in the library to use the specified `assets/` folder.
> `initAssetsDir` is a function that refractor the path to the `assets/` folder for the whole lib.

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

Now you can use the library ! To get start, you can use the `main.py` available on this repo, it's a template you can use as default.

```bash
$ python main.py
# to run your app
```
### From Dataiku

On Dataiku, you will also need an Environnement, to do so, go to Administration > Code Envs (on the top right). You need to request a Python env if you don't have one. 
Once you selected your env, go to General > Extra options section, and add an Extra options for "pip install", and add the `extra-index-url`.

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
- From your project home, hover the `</>` section of the menu and go to webapps.
- Click New Webapp > Code Webapp > Dash.
- In the Settings of this app, make sure the right code env is selected.

You can now start to develop ! you can use the `main.py` available on this repo, it's a template you can use as default.

> [!WARNING]  
> If you haven't defined the assets folder, please make sure to go above at "🛰️ Environnement Variables" section. Otherwise, the images and css won't be applied for the components. 

## 🛠️ Components

### **Navbar**

<details>

<summary>
Most used component, this component is a menu styled following the way of SLB. 
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `title` <sub>string</sub> : The name of your app, it's displayed at the center of the menu.
- `tooltip_users` <sub>array of strings</sub> : List of users that have contribued in any way to the project, when this project is not None, a tooltip icon appear beside the title showing the contributors when hovered.
- `show_toggler` <sub>boolean</sub> : Atiavte a burger button on the right side of the menu, this button is required to integrate the sidemenu component.
- `is_sticky` <sub>boolean</sub> : Make the menu stick to the top when scrolling or not.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Navbar(
    title="AI Lab SRPC App",
    tooltip_users=[
        "First Contributor",
        "Second Contributor",
    ],
    show_toggler=True,
    is_sticky=True,
),
```

</details>

### **Sidebar**

<details>

<summary>
A customizable off-canvas sidebar, designed for adding supplementary menu content or links. 
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `html_content` <sub>HTML or Dash component</sub> : Custom HTML or Dash components to display inside the sidebar.

<br/>

```python
import AI_Lab_DC.components as cp
from AI_Lab_DC.callbacks.sidebar_callback import register_sidebar_callback

cp.Sidebar(
    html_content=html.Div(
        [
            html.H5("Resource Link(s)", style={"text-decoration": "underline"}),
            html.A(
                "AI Lab",
                href="https://slb001.sharepoint.com/sites/e-STICPublic",
                target="_blank",
            ),
            html.Br(),
            html.A(
                "Project Description",
                href="https://slb001.sharepoint.com/sites/e-STICPublic",
                target="_blank",
            ),
            html.Br(),
            html.H5(
                "Author(s)",
                style={"text-decoration": "underline", "margin-top": "10px"},
            ),
            html.A(
                "Put your CNP here",
                href="https://www.eureka.slb.com",
                target="_blank",
            ),
        ]
    )
),

register_sidebar_callback(app) # Connect the Sidebar to the Navbar
```

$${\color{red}[!IMPORTANT]}$$ This component need a callback callable from the lib and used to link the burger button in the Navbar to the Sidebar.

</details>

### **Button**

<details>

<summary>
A customizable button component with various styles, colors, and transition effects
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `button_text` <sub>string</sub> : The text displayed on the button.
- `button_style` <sub>string</sub> : The button's style. Options include:
    - `filled` <sub>(default)</sub> : A solid button style.
    - `bordered` : A button with a bordered style.
    - `mini` : A smaller button variant.
- `button_color` <sub>string</sub> : The button's color. Options include:
    - `primary` <sub>(default)</sub> : SLB blue.
    - `secondary` : Secondary blue.
    - `positive` : Green.
    - `negative` : Red.
    - `deactivate` : Grey.
- `button_transition` <sub>string</sub> : The transition effect when interacting with the button. Options include:
    - `scale` <sub>(default)</sub> : Adds a scaling effect.
    - `fade` : Adds a fading effect.
- `disabled` <sub>boolean</sub> : Disables the button if set to True.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Button(
    button_text="Click Me",
    button_style="filled",
    button_color="primary",
    button_transition="scale",
    disabled=False,
),
```
</details>

### **Checkbox**

<details>

<summary>
A styled checkbox component with customizable labels, alignment, and state control.
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `checkbox_label` <sub>string</sub> : The text label displayed next to the checkbox.
- `checkbox_checked` <sub>int</sub> : Indicates whether the checkbox is checked initially. Use `1` for checked and `0` for unchecked <sub>(default)</sub>.
- `reverse_label` <sub>boolean</sub> : Reverses the order of the label and the checkbox if set to `True`.
- `disabled` <sub>boolean</sub> : Disables the checkbox if set to `True`.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Checkbox(
    checkbox_label="Accept Terms and Conditions",
    checkbox_checked=1,
    reverse_label=False,
    disabled=False,
),
```
</details>

### **Divider**

<details>

<summary>
A horizontal divider component, optionally displaying a label in the center. 
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `label` <sub>string</sub>: Optional text displayed at the center of the divider. If empty, the divider will be a simple line.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Divider(
    label="Section Title",
),
```
</details>

### **Dropzone**

<details>

<summary>
A file upload component with drag-and-drop functionality, customizable labels, and file type restrictions.
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `id` <sub>string</sub>: Unique identifier for the DropZone component.
- `label` <sub>string</sub>: Text displayed inside the drop area. Default is "Drag and drop files here".
- `hint` <sub>string</sub>: Descriptive text displayed below the drop area. Default is a generic example message.
- `file_types` <sub>list of strings</sub>: Restricts the accepted file types. Provide a list of MIME types (e.g., `["image/png", "image/jpeg"]`) or file extensions (e.g., `[".png", ".jpg"]`).

<br/>

```python
import AI_Lab_DC.components as cp
from AI_Lab_DC.callbacks.dropzone_callback import register_dropzone_callback

cp.DropZone(
    id="upload-zone",
    label="Upload your files",
    hint="Accepted formats: PNG, JPEG.",
    file_types=[".png", ".jpg", ".jpeg"],
),

register_dropzone_callback(app, "upload-zone")
```

$${\color{red}[!IMPORTANT]}$$ This component need a callback callable from the lib and used to handle the component once a file is downloaded.

</details>


## 🚶‍♂️ Author

- [@Huang-Frederic](https://github.com/Huang-Frederic)

## 🔗 How to contribute ?

You can extend the project by adding custom components in the components folder. Simply create a function that returns your desired component and include it in the __init__.py file. Similarly, new callbacks can be added to the callbacks folder, following the structure of existing examples. For assets like styles or images, place them in the appropriate subfolder within assets.

To ensure consistency, take inspiration from SLB components, adapting the style with the correct colors and designs referenced in the Figma. Use the Figma as a guide to align your work with the existing design system.

### Resources:
- [Figma Design](https://www.figma.com/design/eYgzX5OvLI1yOr1bmWknvL/DLS-Components?node-id=9-4&node-type=canvas&t=hrfKIRuWDVE5cKPu-0)
- [Asset Resources Platform](https://mediabank.slb.com/portals/cy0aji5n/ImageLibrary)
- [Angular Component Library](https://slb001.sharepoint.com/sites/MH-OurBrand/SitePages/Design-%26-Dev-Toolkit.aspx?xsdata=MDV8MDJ8fGI5MTgzMzcxMDQ5YzQ0YzQyNDMzMDhkY2NlNGZjMzg3fDQxZmYyNmRjMjUwZjRiMTM4OTgxNzM5YmU4NjEwYzIxfDB8MHw2Mzg2MTIwODkxNTc5MTk3OTV8VW5rbm93bnxWR1ZoYlhOVFpXTjFjbWwwZVZObGNuWnBZMlY4ZXlKV0lqb2lNQzR3TGpBd01EQWlMQ0pRSWpvaVYybHVNeklpTENKQlRpSTZJazkwYUdWeUlpd2lWMVFpT2pFeGZRPT18MXxMMk5vWVhSekx6RTVPbTFsWlhScGJtZGZXbTFOZWs5RVRtbE5hbWQwVFcxWk5WcFRNREJOYWtsM1RGUnNiRTB5VlhSWk1rVXhXVEpKZVU1SFNYZFBSRkp0UUhSb2NtVmhaQzUyTWk5dFpYTnpZV2RsY3k4eE56STFOakV5TVRFMU1qSXh8NmE1MDEzOTc1MjE1NGYzODI0MzMwOGRjY2U0ZmMzODd8NjMxOGY4YjRiNDNkNDRlN2FhYjBiZjI1YWE2ZGE1YmM%3D&sdata=WG83Vks5M2hkcVd2bVpoMEN2dzJJWGV5Y0p6dVpBQXNKVkJyNTZXTnpYbz0%3D&ovuser=41ff26dc-250f-4b13-8981-739be8610c21%2CFHuang5%40slb.com&OR=Teams-HL&CT=1725612124769&clickparams=eyJBcHBOYW1lIjoiVGVhbXMtRGVza3RvcCIsIkFwcFZlcnNpb24iOiIxNDE1LzI0MDgwMjEyMDA4IiwiSGFzRmVkZXJhdGVkVXNlciI6ZmFsc2V9)

Artifacts are managed by the pipeline, so any changes pushed to the main branch will automatically update the project. If you're making significant updates, remember to modify the version in the pyproject.toml file to reflect the changes.
