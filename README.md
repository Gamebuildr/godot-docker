# Godot Dockerized

This repo contains Docker images custom built with the [Godot Engine](https://godotengine.org/) to be used in conjuction with the [Gamebuildr CI](https://www.gamebuildr.io) system. They are created to be used with Godot headless server binaries and will contain support for all platform building (planned targets are iOS, Steam, Android, and consoles).

We currently support legacy versions of Godot with version 2.1.4 (and potentially 2.1.5). We will offer full support for Godot 3.x and beyond.

# Building the docker image

For simplicity, the Dockerfiles provided also come paired with docker-compose files, which means you only need to run ```docker-compose up``` to build the image and then start a new docker container. After the conatiner is built and is running you can ssh into the instance by running:

```
docker exec -it <name-of-docker-image> bash
```

If you need to recompile the image at any time, simple run ```docker-compose build``` and the image will be re-created. In addition, you can always build the image without docker compose by running the standard docker build command.

Docker compose also exposes a context directory where you can place games that you're working on. This will automatically share the folder with the docker container and allow you to test build commands against.

# Godot build specifics

Godot has a variety of build commands and specifications available to run and it takes a lot of trial and error to figure this out. Since we've already done a good bit of work on this, we'll outline what steps to take and helpful hints.

### Export Templates

In short, export templates are used to run generic builds for games built in Godot. Namely Windows, OSX, Linux/X11, and HTML.

Templates can be downloaded from [Godot Downloads](https://godotengine.org/download/osx). The hardest part about installing templates (without an associated GUI) is figuring out where to put templates in Linux (Mac OSX and Windows have different paths which we won't cover here).

For Godot 2.x export templates can be placed in a .godot folder located at ```~/.godot/<exported template files>```. Fairly straightforward.

For Godot 3.0.x export templates must be placed in a folder located at ```~/.local/share/godot/templates/3.0.1.stable/```. At this point you might be asking **where the hell did I get the name 3.0.1.stable at**. 

And the simple answer is you need to first download the templates, extract the archive (using standard archive utility), open up the folder called version.txt and see what the file contains. It should simply contain ```3.0.1.stable``` or whatever version of the template. Then use this version name as the folder name in the created location at ```~/.local/share/godot/templates/<version_name>```. Simple, right?

In general you can assume the folder name is called ```GODOT_VERSION-stable```. If you're getting an error about export templates not being found, then make sure this folder matches with the name inside version.txt

### Godot 2.x

Headless builds in Godot 2.x are fairly straightforward, the command can be structured as:

```
godot -export 'Mac OSX' /context/build.zip -path /project/directory -v
```

### Godot 3.0.x

> C# exports are broken for versions 3.0, 3.0.1, and 3.0.2. C# exports are expected to work in 3.1
> We also only support version 3.0.1 and upwards since headless server building was broken in 3.0

To run an export you have to structure the command in a specific way, for example:

```
godot --export-debug "Mac OSX" /compiled/target/name.zip --path /path/to/project/ --build-solutions --quit -v
```

The commands we use are listed below:

* ```--export-debug```: outputs helpful commands throughout the process to let you know where the command is failing

* ```--export```: Normal command to signify you want to export a game.

* ```location```: /target/location/ means the output file you want to expor to. Must contain a file that has the .zip extension

* ```--path```: Path to your Godot project, must specify a directory, and the directory must contain a project.godot file

* ```--build-solutions```: Compiles a project that takes advantage of C# and mono, not needed if you're not using C#

* ```--quit```: For closing the editor when using export via headless server builds, otherwise the editor will open

* ```-v```: Verbose output to help better understand what's going on and debug

If you're ever unsure about what commands you can run or what to know more about the cli just type in ```godot -h``` which will print the help text of all the commands to your console. I literally found this by accident.
