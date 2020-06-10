**Image** is a template.
- To download it we use `pull` command  
- You can't delete image until there are any corresponding containers created from this image.
- If **container** runs an application , **image** has OS and all application files
- Images don't have a kernel so they share kernel of the host machine
- image pull needs repository name and tage name `ubuntu:latest`
All Docker images start with a base layer and as changes are mode new layers are added   
For example: First image layer is ubuntu , then you add Python package and it will be added as a second layer
