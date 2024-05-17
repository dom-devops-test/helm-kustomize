# Helm and Kustomize

This repo contains examples on the uilisation of Kustomize. Some basic Kustomize examples are given for the basic understanding of the tool and it's functionalities. Other examples contains different usage of Kustomize in conjuncture with Helm to demonstrate the capabilities and advantages of combining both tools together.

## Installation of the tools

* To install Helm refer to the [official site's documentation](https://helm.sh/docs/intro/install/).
* To install Kustomize refer to the [official site's documentation](https://kubectl.docs.kubernetes.io/installation/kustomize/).

## Usage

To make Kustomize render the application templates that will be generated, use
```
kustomize build <path-to-kustomization.yaml>
```

To deploy you deployment using kustomize, use
```
kustomize build <path-to-kustomization.yaml> | kubectl apply -f -
```

If you are using multi level directories and child directories, you need to add the ```--load-restrictor``` flag to your command

```
kustomize build --load-restrictor=LoadRestrictionsNone <path-to-kustomization.yaml>
```

In case you are using Kustomize with Helm the ```--enable-helm``` flag needs to be added too.

```
kustomize build \
    --enable-helm \
    --enable-alpha-plugins \
    --load-restrictor=LoadRestrictionsNone <path-to-kustomization.yaml>
```

# Content

Every repo has been made to illustrate differents capabilities.

## [kustomize-demo](./kustomize-demo/)

This repository was made to illustrate the basic use of Kustomize. You can see the basic structure and usage of the base, overlays and different kind of patches you can apply.</br>

```
|_base -> contains all the base yaml files and the base kustomization file.
|
|_overlays -> contains all overlays directories for specific envs.
   |
   |_dev   -> demonstrates a patch using a yaml file
   |_pprod -> demonstrates a patch using an inline string
   |_prod  -> demonstrates a patch using PatchesJson6902
```

## [kustomize-demo-with-layers](./kustomize-demo-with-layers/)
This repository was made to illustrate use of Kustomize with environements and sub-directories. You can see here that you need to create a folder for your top-level tenant containing the kustomization file (refering to all your manifest files in the base directory) and where we are applying some patches. We then need to create a tenant directory to overide this base directory since we can't refer to a folder containing sub-diectories with kustomization files from our refered kustomization. In other words, the directory where you are pointing to create your kustomization.yaml must contain a maximum of one kustomization.yaml in the folder or it's sub directories.<br>

```
|_base -> contains all of our manifest yaml files.
|
|_overlays -> contains all overlays directories for specific envs.
   |
   |_dev   -> contains our root-tenant and tenants
      |
      |_root-tenant  -> our root tenant's kustomization (we applied patches)
      |_tenants      -> our tenants directory 
         |
         |_bob-team  -> our kustomization where we apply a patch on the patch already applied from our root-tenant
         |_dan-team  -> our kustomization where we apply a patch on the patch already applied from our root-tenant
            |
            |_dan-child -> this is a sub-directory referencing dan-team's kustomization 
```

<br>
Here I added a child directory to dan-team to show it is not possible to refer to a base directory containing sub-directories with other kustomization files.<br>
The patches used in these examples show how it is possible to use $patch: delete or to patch by adding values to a previous patch in case of an array.<br>

Please remember that when using directories, sub-directories and other external or multi-references, you need to add the ```--load-restrictor``` flag to your kustomize build command.

```
kustomize build --load-restrictor=LoadRestrictionsNone <path-to-kustomization.yaml>
```

## [kustomize&helm-demo](./kustomize&helm-demo/)

This repository was made to demonstrate the use of Helm in the kustomization files.

```
|_manifests -> directory that will contain all our helm charts directories. Note that you can remove this and create a Helm repository
   |
   |_custom-webapp -> our custom webapp chart with it's predefined default values
   |_releader      -> a chart existing for the reloader app. The values can be modified here if we want to customize the base.
|
|_environment/tenants -> directory containing our environments sub-directories
   |
   |_dev -> our dev envrionment containing our application directories for dev
   |  |
   |  |_app1  -> our kustomization file and files required for our app1 deployment
   |  |_app2  -> our kustomization file and files required for our app2 deployment
   |
   |_qa  -> our qa envrionment containing our application directories for qa
      |
      |_app1  -> our kustomization file and files required for our app1 deployment
      |_app2  -> our kustomization file and files required for our app2 deployment
```

<br>

Please remember that when using Helm with kustomize, the ```--enable-helm``` flag needs to be added to our kustomize build command.

## [kustomize&helm-folder-structure](./kustomize&helm-folder-structure/)

This repository contains the structure I used in my project. I built it very basically just to show the approach taken. This is just an example as it was requested. I tried to add some readmes and comments in some files to explain the structure.  Feel free to take a look and if you have any other questions you can message me.

## Author

Dominique Hereish - dominique.hereish@cgi.com

## References

* [Helm official documentation](https://helm.sh/docs/)
* [Kustomize official documentation](https://kustomize.io/)

