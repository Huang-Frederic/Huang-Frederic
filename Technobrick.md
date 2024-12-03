
# 📍 Embedded AI Lab Dash Template

This is a Template that you can clone to start your Dash project, it will contain all the components needed for a fast and easy release.

## 🔮 Stack

![Static Badge](https://img.shields.io/badge/python-gray?style=for-the-badge&logo=Python)
![Static Badge](https://img.shields.io/badge/dash-gray?style=for-the-badge&logo=Dash)
![Static Badge](https://img.shields.io/badge/plotly-gray?style=for-the-badge&logo=Plotly)
![Static Badge](https://img.shields.io/badge/dataiku-gray?style=for-the-badge&logo=Dataiku)

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

> From your project home, hover the "</>" section of the menu and click on Libraries (or type G+L).
> On the top right you have a section called resources you should see a folder called lib/local-static, if not, create the folder.
> Upload the entire assets folder of this repo in the local-static folder.
> Go on your main.py, and rather that define the assets_folder during the creation of the app such as above, define it as following. 

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

> [!WARNING]  
> If your Supabase API calls fail or return empty results, it's probably because we have not implemented RLS, you can disable it by running the following query in the SQL Editor of your project on Supabase.


These environnement variables are required for postgres integrations (you can find all these informations after creating a project in Supabase :

```env
DB_USER = "db_user"
DB_PASSWORD = "db_password"
DB_HOST = "db_host"
DB_PORT = "db_port"
DB_NAME = "postgres"
```

## 💻 Installation

### From source

> Clone the repository

```bash
$ git clone https://github.com/Huang-Frederic/RRW-Internship
$ cd RRW-Internship
```

> Install Python Dependencies

```bash
$ cd script
$ python -m venv env
$ env/bin/activate  # On Windows, use `env\Scripts\activate`
$ pip install --no-deps -r requirements.txt
```

> Create a Supabase Project

```sql
# Create a new project in Supabase and create a table called "companies" with the following properties :

create table companies (
  id uuid primary key,
  name text not null,
  description text,
  country text,
  funding_amount numeric,
  industries text[]
);
```

> Set Up Environment Variables

```bash
# Create a .env file in the root directory of your project and add the following environment variables

SUPABASE_URL = "https://your_url.supabase.co"
SUPABASE_KEY = "your secret key"
DB_USER = "db_user"
DB_PASSWORD = "db_password"
DB_HOST = "db_host"
DB_PORT = "db_port"
DB_NAME = "postgres"
```

> Run the script

```bash
$ python script.py

# -> And now you should see the values in your database
```

> [!WARNING]  
> If your Supabase API calls fail or return empty results, it's probably because we have not implemented RLS, you can disable it by running the following query in the SQL Editor of your project on Supabase.

```sql
ALTER TABLE companies DISABLE ROW LEVEL SECURITY;
```

> Install Nuxt.js Dependancies

```bash
$ cd ../web-app
$ npm install
```

> Run the project

```bash
$ npm run dev

# http://localhost:3000
```

## 🚶‍♂️ Author

- [@Huang-Frederic](https://github.com/Huang-Frederic)

## 🔗 Acknowledgements

Red River West Recruitment :

- [Technical Test Instructions](https://redriverwest.notion.site/Fullstack-Developer-Internship-Take-home-technical-test-059113beb6d549e49baffc50b7500bdd)
- [Imposed Design](https://www.figma.com/design/ZpGmtyYlHS344OI8J6cD79/Untitled?node-id=0-1)

Usefull Documentations :

- [Nuxt.JS](https://nuxt.com/?uwu=true)
- [Supabase](https://supabase.com/)
