# RStudio as an Open OnDemand App

This repository provides an interactive Open OnDemand app that launches an RStudio Server using a Singularity image. The app is based on [bc\_osc\_rstudio\_server](https://github.com/OSC/bc_osc_rstudio_server) and [ood-bih-rstudio-server](https://github.com/bihealth/ood-bih-rstudio-server).

## Create your own RStudio App 

#### 1. Clone this repository
Clone this repository into your `~/ondemand/dev/` directory with `git clone`. 
<br/>
<br/>

#### 2. Import RStudio Singularity Image 
You can download pre-built RStudio Singularity images from the [Rocker Project](https://rocker-project.org/). To get the latest version, navigate to the `~/ondemand/dev/ood_haas_rstudio_server/singularity_images/` directory and run:
<br/>
```
singularity pull docker://rocker/rstudio:latest
```

Rename the `.sif` file so that it indicates the RStudio version. You can also build your own singularity image and customize it to your liking. 
<br/>
<br/>

#### 3. Change the app template files 
Once you've added a new singularity image, you have to change the template files that are used to run RStudio from the image. 

- The `form.yml` file specifies the launch site for the RStudio server. Under `singularity_image:`, add a new option to add your image to the drop down menu. The first string will appear in the OnDemand frontend, the second string is the file name of your image: `- [ "displayed_text", "rstudio_version.sif" ]`. You can also adapt the other specifications.

- The `manifest.yml` file manages the name and description of your app, and you can upload another icon.png.

- The `submit.yml.erb` and `view.html.erb` files handle the cluster specifications and sign-in. You don't need to change anything here, but you can.

- In `template/before.sh.erb`, change the path to the `RSTUDIO_SERVER_IMAGE` so that it points to your image. Alternatively, you can point to a folder containing several images, and pass the `.sif` file name from the `form.yml` options with `<%= context.singularity_image %>`:
  ```
  /path/to/singularity_images/<%= context.singularity_image %>
  ```
  Relative paths are not recognized sometimes, so stick to absolute paths if you run into problems.
  
- `template/after.sh.erb` handles the start-up message for the RStudio server instance.

- Finally, `template/script.sh.erb` starts the RStudio server from the singularity image. You can customize the `apptainer run` command, R library paths, miniconda and more. Here you can also explicitly load libraries such as GNU C++ from a miniconda environment:
  ```
  export LD_PRELOAD=/fast/work/users/<user_name>/bin/miniconda3/envs/<env>/lib/libstdc++.so.6
  ```
<br/>

#### 4. Start RStudio
Restart the [hpc-portal.cubi.bihealth.org](hpc-portal.cubi.bihealth.org) web server to add your sandbox app and start a RStudio server instance. 
<br/>
<br/>

## License

MIT | Copyright (c) 2016-2018 Ohio Supercomputer Center
