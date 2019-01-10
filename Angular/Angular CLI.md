# Angular CLI

```
npm install -g @angular/cli
```

# Angular CLI workspace file (angular.json) schema(https://github.com/angular/angular-cli/wiki/angular-workspace)

```
{
    varsion:"1",
    newProjectRoot:
    defaultProject
    cli:{
        defaultCollection
        packageManager
        warnings:{
            versionMismatch
            typescriptMismatch
        }
    },
    schematics
    projects:{
        projectName:{
            root:	//root directory of project
            sourceRoot: //The root of the source files, assets and index.html file structure
            projectType:	application or library
            prefix:  //The prefix to apply to generated selectors.
            schematics:{
                
            }
            architect:{
                
            }
        }
    }
}
```



## ng new {name}(https://github.com/angular/angular-cli/wiki/new)

Default applications are created in a directory of the same name, with an initialized Angular application

Options

​	--routing		: Generates a routing module

​	--prefix		:The prefix to apply to generated selectors.

​	--style		:The file extension to be used for style files.

​	--skip-tests/-s:Skip creating spec files.



# ng serve

```
ng serve [project]
```

Options 

​	--prod

​	--configuration

--browser-target

--port

--host

--proxy-config

--progress

--optimization

# ng generate 

1. **class**

   `--spec`

    --type

   --project

   --force

   --dry-run

2. **component**, **directive**

   --inline-style

   --inline-template

   --change-detection

   **--prefix**	

   **--styleext**			The file extension to be used for style files.

   **--spec**

   --flat			Flag to indicate if a dir is created.

   --skip-import

   **--selector**

   --module

   **--export**		Specifies if declaring module exports the component.

3. guard

4. enum

5. interface

6. module

7. pipe

8. service

9. application

10. library

11. universal