
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

 Default 

By default, Dash manages most configurations automatically, and you can define the assets_folder as follows:

```python
app = Dash(
    __name__,
    external_stylesheets=[dbc.themes.BOOTSTRAP],
    assets_folder="AI_Lab_DC/assets",
)
```

 Dataiku

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

 From source

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
 From Dataiku

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
- Other attributes are passed directly to the `dbc.Navbar` component.

<ins>**__Structure__**</ins>:

- `Left Section`: Displays the logos "SLB" and "Embedded AI Lab".
- `Center Section`: Displays the title of the app and the contributors in a tooltip (optional).
- `Right Section`: Displays a clickable burger menu .

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
The Sidebar component is a collapsible container, typically used for navigation or additional controls. It can display a combination of interactive or static content, such as links, buttons, or other UI elements, arranged vertically. The component supports dynamic updates and integrations with Dash callbacks, making it versatile for both simple and complex Dash applications.
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `html_content` <sub>HTML or Dash component</sub> : Custom HTML or Dash components to display inside the sidebar.
- Other attributes are passed directly to the `dbc.Offcanvas` component.

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

<ins>**__Callback__**</ins> : 
- `app` : Put the app in argument to keep the callback on the loop. 

</details>

### **Button**

<details>

<summary>
The Button component provides a customizable and interactive button element for triggering actions or navigating within a Dash application. It supports multiple styles, such as filled, bordered, and mini, with a choice of colors (e.g., primary, secondary, etc.) to match your design needs. The button also includes optional animated transitions, like scaling or fading effects, to enhance user engagement. It is suitable for use in forms, dashboards, and general interactivity.


    
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
- Other attributes are passed directly to the `html.Button` component.

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
The Checkbox component is a versatile and visually appealing way to capture binary input (checked/unchecked) from users. It can be used for single or multiple selections in forms, settings, or any interactive interface. The component supports a reversed label layout and integrates seamlessly with Dash callbacks for state management. Additional styling options allow for consistent theming, and it includes support for a disabled state when input is restricted.

    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `checkbox_label` <sub>string</sub> : The text label displayed next to the checkbox.
- `checkbox_checked` <sub>int</sub> : Indicates whether the checkbox is checked initially. Use `1` for checked and `0` for unchecked <sub>(default)</sub>.
- `reverse_label` <sub>boolean</sub> : Reverses the order of the label and the checkbox if set to `True`.
- `disabled` <sub>boolean</sub> : Disables the checkbox if set to `True`.
- Other attributes are passed directly to the `dbc.Checklist` component.

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
The Divider component is a horizontal separator, used to structure content within an application. It include an optional central label, allowing developers to group related content meaningfully. The simple yet flexible design ensures that the divider integrates naturally into various layouts, helping to visually organize and enhance readability within complex dashboards.    
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `label` <sub>string</sub> : Optional text displayed at the center of the divider. If empty, the divider will be a simple line.
- Other attributes are passed directly to the `html.Div` component.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Divider(
    label="Section Title",
),
```
</details>

### **Dropdown**

<details>

<summary>
The Dropdown component provides an interactive, user-friendly dropdown menu for selecting options from a list. It supports labels, placeholder text, and an optional hint for improved accessibility. The component is fully customizable, allowing for disabled states, dynamic updates, and integration with Dash callbacks. 
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `options` <sub>list of dictionaries</sub> : Specifies the dropdown options. Each dictionary includes:
    - `label` <sub>string</sub> : The display text for the option.
    - `value` <sub>any</sub> : The value assigned to the option.
- `label` <sub>string</sub> : A descriptive label displayed above the dropdown.
- `hint` <sub>string</sub> : Additional information displayed below the dropdown.
- `disabled` <sub>boolean</sub> : Disables the dropdown if set to `True`.
- `placeholder` <sub>string</sub> : Placeholder text displayed when no option is selected.
- Other attributes are passed directly to the `dcc.Dropdown` component.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Dropdown(
    options=[
        {"label": "Option 1", "value": "option1"},
        {"label": "Option 2", "value": "option2"},
        {"label": "Option 3", "value": "option3"},
    ],
    label="Select an option",
    hint="Choose an option from the dropdown menu.",
    placeholder="Select...",
    disabled=False,
    className="custom-dropdown-class",
)
```

</details>

### **Dropzone**

<details>

<summary>
The DropZone component enables intuitive file uploads through drag-and-drop functionality. Users can drag files onto the designated area, or optionally click to open a file selection dialog. With support for customizable labels, hints, and file type restrictions, the DropZone offers flexibility for a variety of use cases, such as uploading images, documents, or data files. It integrates seamlessly with Dash callbacks, enabling developers to process uploaded files dynamically.    
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `id` <sub>string</sub> : Unique identifier for the DropZone component.
- `label` <sub>string</sub> : Text displayed inside the drop area. Default is "Drag and drop files here".
- `hint` <sub>string</sub> : Descriptive text displayed below the drop area. Default is a generic example message.
- `file_types` <sub>list of strings</sub> : Restricts the accepted file types. Provide a list of MIME types (e.g., `["image/png", "image/jpeg"]`) or file extensions (e.g., `[".png", ".jpg"]`).
- Other attributes are passed directly to the `dcc.Upload` component.

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

<ins>**__Callback__**</ins> : 
- `app` : Put the app in argument to keep the callback on the loop. 
- `dropzone_id` : Define the dropzone component of this action.

</details>

### **Footer**

<details>

<summary>
The Footer component is a responsive, multi-section footer designed for branding and navigation. It typically includes three sections: branding or credits, a central message, and an optional link to external resources like SharePoint or documentation. The component ensures consistency across pages and supports dynamic updates for real-time customization. Its structured layout enhances the professional appearance of your application.    
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `sharepoint_link` <sub>string</sub> : URL for the SharePoint link displayed in the footer.
- Other attributes are passed directly to the `dbc.Container` component.

<br/>

<ins>**__Structure__**</ins>:

- `Left Section` : Displays the text "Powered by -" followed by the AI Lab logo and "Embedded AI Lab SRPC".
- `Center Section` : Displays the classification "SLB-Private".
- `Right Section` : Displays a clickable link to SharePoint, with accompanying SharePoint logo.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Footer(
    sharepoint_link="https://sharepoint.example.com",
),
```

</details>

### **Input**

<details>

<summary>
The Input component is a flexible, multi-purpose input field for capturing user data. It supports different types (text, textarea, and numeric), with options for prefixes, suffixes, and additional control buttons (e.g., increment/decrement for numeric inputs). Customizable labels and hints enhance usability, while seamless integration with Dash callbacks ensures dynamic interaction and real-time feedback. It is ideal for forms, data entry, and interactive dashboards.
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `type` <sub>string</sub> : Specifies the input type. Options include:
    - `"text"` <sub>(default)</sub> : A standard text input.
    - `"textarea"` : A multi-line text input.
    - `"numeric"` : A number input with increment and decrement buttons.
- `placeholder` <sub>string</sub> : Placeholder text displayed inside the input field.
- `prefix` <sub>string</sub> : Optional text displayed before the input field for text type inputs.
- `suffix` <sub>string</sub> : Optional text displayed after the input field for text type inputs.
- `label` <sub>string</sub> : A descriptive label displayed above the input field.
- `hint` <sub>string</sub> : Additional information displayed below the input field.
- `disabled` <sub>boolean</sub> : Disables the input if set to True.
- Other attributes are passed directly to the `dbc.Input` component.

<br/>

```python
import AI_Lab_DC.components as cp

# Example for text input
cp.Input(
    type="text",
    placeholder="Enter your name",
    prefix="Name",
    label="User Name",
    hint="Please enter your full name.",
    disabled=False,
),

# Example for numeric input
cp.Input(
    type="numeric",
    placeholder="Enter a number",
    label="Age",
    hint="Use the buttons to adjust the value.",
    disabled=False,
    id="numeric-input-example",
),
```

</details>

### **Radio**

<details>

<summary>
The Radio component presents a group of radio buttons for single-option selection. It supports dynamic styling, labels, and tooltips for enhanced user guidance. Each option can display supplementary information using interactive tooltips, making it ideal for complex configurations. The component integrates with Dash callbacks, ensuring that user selections can dynamically update the application state.
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `options` <sub>list of dictionaries</sub> : Specifies the radio button choices. Each dictionary can include :
    - `label` <sub>string</sub> : The display text for the option.
    - `value` <sub>any</sub> : The value assigned to the option.
    - `tooltip` <sub>string</sub> : (Optional) Tooltip text shown when hovering over the info icon.
- `label` <sub>string</sub> : A descriptive label displayed above the radio items group.
- `hint` <sub>string</sub> : Additional information displayed below the radio items group.
- Other attributes are passed directly to the `dcc.RadioItems` component.

<br/>

```python
import AI_Lab_DC.components as cp

cp.RadioItems(
    options=[
        {"label": "Option 1", "value": "option1", "tooltip": "This is Option 1."},
        {"label": "Option 2", "value": "option2", "tooltip": "This is Option 2."},
        {"label": "Option 3", "value": "option3"},  # No tooltip
    ],
    label="Choose an option",
    hint="Hover over the info icons to learn more.",
    value="option1",  # Default selected option
    className="custom-radio-group",
),
```
</details>

### **Slider**

<details>

<summary>
The Slider component provides an interactive control for selecting a value or range within a defined range. It supports customizable templates, vertical or horizontal orientation, and optional marks for clear value indication. Designed for scenarios like filtering data, adjusting parameters, or fine-tuning settings, the component integrates seamlessly with Dash callbacks for responsive and dynamic behavior.
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `max_value` <sub>number</sub> : Maximum value of the slider.
- `min_value` <sub>number</sub> : Minimum value of the slider (default is `0`).
- `start_value` <sub>number</sub> : Initial value of the slider (defaults to `min_value`).
- `step_value` <sub>number</sub> : Increment step for the slider (default is `1`).
- `template` <sub>string</sub> : Formatting string for the tooltip (e.g., `"{value}%"`).
- `is_vertical` <sub>boolean</sub> : Determines the slider's orientation (default is horizontal).
- Other attributes are passed directly to the `dcc.Slider` component.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Slider(
    max_value=100,
    min_value=0,
    start_value=50,
    step_value=5,
    template="{value}%",
    is_vertical=False,
    id="example-slider",
),
```

</details>

### **Switch**

<details>

<summary>
The Switch component is a  toggle control for binary input. It supports labeled options, reversed layouts, and a disabled state for restricted input. Its compact design makes it suitable for settings, filters, and on/off controls in dashboards or forms. The component integrates with Dash callbacks for real-time interaction.
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `switch_label` <sub>string</sub> : Label text displayed alongside the switch.
- `switch_checked` <sub>integer</sub> : Sets the initial state of the switch (default is `0` for unchecked).
- `reverse_label` <sub>boolean</sub> : If `True`, places the label to the right of the switch (default is `False`).
- `disabled` <sub>boolean</sub> : Disables the switch when set to `True` (default is `False`).
- Other attributes are passed directly to the `dbc.Checklist` component.

<br/>

```python
import AI_Lab_DC.components as cp

cp.Switch(
    switch_label="Enable Notifications",
    switch_checked=1,
    reverse_label=False,
    disabled=False,
    id="example-switch",
),
```

</details>

### **TabControl**

<details>

<summary>
The TabControl component is a collapsible UI element that toggles between showing and hiding its content when clicked. It provides an interactive way to organize and display content sections dynamically.
    
</summary>
<br/>

<ins>**__Parameters__**</ins> : 
- `tab_id` <sub>string</sub> : Unique identifier for the tab. Used for button and content container IDs.
- `label` <sub>string</sub> : Text displayed on the tab's toggle button.
- `content` <sub>component, optional</sub> : The content to display inside the tab when expanded. Defaults to `html.Div("This is the default content.")`.
- `is_open` <sub>boolean, optional</sub> : Initial state of the tab. If `True`, the tab starts expanded; otherwise, it's collapsed. Defaults to `False`.

<ins>**__Structure__**</ins> : 
- `Toggle Button` : A button with an arrow icon (`▶` for closed and `▼` for open) followed by the `label`. Clicking toggles the visibility of the `content`.
- `Content Area` : Contains the content passed to the `content` parameter. Hidden or shown based on the state of the toggle button.

<br/>

```python
import AI_Lab_DC.components as cp
from AI_Lab_DC.callbacks.tabcontrol_callback import register_tabcontrol_callback

example_content = html.Div(
    "This is example content for the tab.", style={"padding": "10px"}
)

cp.TabControl(
    tab_id="tabcontrol_id",
    content=example_content,
    is_open=True,
),

register_tabcontrol_callback(app, "tabcontrol_id", label="Example Tab")
```

$${\color{red}[!IMPORTANT]}$$ This component need a callback callable from the lib and used to handle the component once a file is downloaded.

<ins>**__Callback__**</ins> : 
- `app` : Put the app in argument to keep the callback on the loop. 
- `tab_id` : Define the tabcontrol component of this action.
- `label` : We need to specify the label to keep it rendered with the right arrow.

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
