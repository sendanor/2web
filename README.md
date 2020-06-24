# 2web

Fast &amp; Secure Website Builder

### Install

This program is still under development and in a design phase.

### Features 

 * Very much similar to [Jekyll](https://jekyllrb.com/) -- but not compatible
 * Secure to run for untrusted build data. Templates are compiled inside Jinja's sandbox and includes do not allow relative paths.
 * Designed KISS (Keep It Simple, Stupid) in mind, without the stupid part.
 * Fast to recompile -- Uses same strategies as Makefiles use
 * Small dependency footprint -- Written in [Python](https://www.python.org/) with [Jinja templates](https://jinja.palletsprojects.com/en/2.11.x/)
 * Template files are 100% compatible with Ansible

### Design 

This compiler is designed to be extremely simple and safe to run for untrusted input data.

It uses [Jinja templates](https://jinja.palletsprojects.com/en/2.11.x/) inside a sandbox as the template engine.

It's perfect solution to compile static websites, but could be used to compile many other content.

#### Data Files

You place your data files under `_data` folder.

For multiple builds with different data, eg. different translations, you may 
find it useful to put your data files in `_data/LANG` and simply change the 
data folder location when compiling.

The defaults will be read from `_data/_default.json` and the optional page 
specific data from `_data/PAGE.json`, where `PAGE` is the name from your 
`_pages/PAGE.EXT` template file.

The resulting data will be passed to the template engine as variables.

#### Template files

Templates for different pages are placed in `_pages/PAGE.EXT` where EXT is 
usually `html`, but can be anything. 

Templates may use data from the `_data` folder. 

For example if you have a page `_pages/index.html`:

 * Variables for the template engine will be read from `_data/_default.json` and `_data/index.json`
 * The compiled result will be saved as `docs/index.html`
 * The file are recompiled only if input files have a newer timestamp than the compiled output.

#### Include files

Templates may use include files from `_includes/NAME`, eg:

For example a file `_includes/header.html` can be included as:

```
{{ include('header.html') }}
```

**Note!** You cannot use relative paths for security reasons.

#### The output directory

Output files are saved `_site/PAGE.EXT` by default.

You may place any uncompiled public files there, too. Just make sure you *don't* use same names for pages, ***or they will get overwritten***.

### Usage

 0) *Optional:* Get coffee/tea beforehand because you most likely will not have 
    enough spare time to get a coffee between these steps.

 1) Change your directory to the folder where your files are located

 2) Run `2web`

 3) *Optional:* Make new changes and run `2web` again

### Command Line Options

| Short     | Long version      | Description                           | Default        |
| --------- | ----------------- | ------------------------------------- | -------------- |
| `-d DIR`  | `--data=DIR`      | You may change the data directory     | `_data`        |
| `-i DIR`  | `--includes=DIR`  | You may change the include directory  | `_includes`    |
| `-t DIR`  | `--templates=DIR` | You may change the template directory | `_templates`   |
| `-o DIR`  | `--output=DIR`    | You may change the output directory   | `_site`        |
| `-c FILE` | `--config=FILE`   | You may change the config file        | `_config.json` |

**Note!** The command line arguments may use relative paths.

### Configuration file

The configuration file `_config.json` can be used to change defaults: 

```
{
  "2web": {
    "data": "data",
    "includes": "includes",
    "templates": "templates"
    "output": "public"
  }
}
```

**Note!** These options **may not** use relative paths (eg. `../foo.txt`).
