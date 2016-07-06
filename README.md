## This plugin is no longer maintained.

### Gradle plugin for integrating the Bootstrap Framework and Font Awesome

[ ![Download](https://api.bintray.com/packages/kensiprell/gradle-plugins/bootstrap-framework/images/download.svg) ](https://bintray.com/kensiprell/gradle-plugins/bootstrap-framework/_latestVersion)

[Bootstrap](http://getbootstrap.com) bills itself as "the most popular HTML, CSS, and JS framework for developing responsive, mobile first projects on the web."

[Font Awesome](http://fortawesome.github.io/Font-Awesome/) is the "iconic font and CSS toolkit."

If you have a question, suggestion, or want to report a bug, please submit an [issue](https://github.com/kensiprell/bootstrap-framework/issues). I will reply as soon as I can.

### Highlights

* The Bootstrap version can be configured in your application's ```build.gradle``` file, which means you do not have to install a different version of the plugin to get a different Bootstrap version, nor do you have to wait on a plugin update to use the latest Bootstrap release.

* The plugin can also manage Font Awesome files, including LESS support.

* The plugin supports the [asset-pipeline-core](https://github.com/bertramdev/asset-pipeline-core) plugin and its [less-asset-pipeline](https://github.com/bertramdev/less-asset-pipeline) module out of the box.

### Sample Application

This Grails [sample application](https://github.com/kensiprell/bootstrap-framework-sample) demonstrates how to use the plugin.

### Installation

Add the following lines to your application's ```build.gradle``` file. The commented-out lines show the plugin default values, and if you are using Grails, they are not required for the plugin to work. See the [Configuration Options](https://github.com/kensiprell/bootstrap-framework#configuration-options) section for the details.

    buildscript {
        ext {
             //bootstrapFramework = [
             //    version             : "3.3.5",
             //    cssPath             : "grails-app/assets/stylesheets",
             //    jsPath              : "grails-app/assets/javascripts",
             //    useIndividualJs     : false,
             //    useLess             : false,
             //    invalidVersionFails : false,
             //    fontAwesome : [
             //       install             : false
             //       version             : "4.3.0",
             //       useLess             : false
             //       invalidVersionFails : false
             //    ]
             //]
        }
        repositories {
            jcenter()
        }
        dependencies {
            classpath "com.siprell.plugins:bootstrap-framework:1.0.3"
        }
    }

    apply plugin: "com.siprell.plugins.bootstrap-framework"
        
### Configuration Options

The following options can be configured in the ```bootstrapFramework``` Map.

#### bootstrapFramework.version

Use the property below to change the Bootstrap Framework version used by your application.

    version : "3.3.1"

#### bootstrapFramework.cssPath

Use the property below to define the location where the Bootstrap CSS, fonts, and LESS files are copied.  
    
    cssPath : "src/main/web-app/resources/css"
    
#### bootstrapFramework.jsPath
    
Use the property below to define the location where the Bootstrap JavaScript files are copied.  
    
    jsPath : "src/main/web-app/resources/js"
    
#### bootstrapFramework.useIndividualJs

If the property below is set to ```true```, the plugin will copy all individual JavaScript files to ```"${bootstrapFramework.jsPath}/bootstrap"```. Otherwise, it will only copy the ```bootstrap.js``` file to the directory.
    
    useIndividualJs : true
    
#### bootstrapFramework.useLess

If the property below is set to ```true```, the plugin will copy all Bootstrap Framework LESS and mixin files to ```"${bootstrapFramework.cssPath}/bootstrap/less"```.

    useLess : true
    
#### invalidVersionFails

The ```invalidVersionFails``` property can be configured individually for ```bootstrapFramework``` and ```bootstrapFramework.fontAwesome```. When the property is at its default of ```false``` and you have entered an invalid version number, the plugin will search the ```build/tmp``` directory for other versions of the appropriate zip file and use the one with the highest version number. It will also display a warning message in the console.  

You can disable this behavior by setting this property to ```true```. If the plugin cannot download the version you specified, it will throw an ```org.gradle.api.InvalidUserDataException```.

    bootstrapFramework = [
        invalidVersionFails : true
        fontAwesome : [
            install : true,
            invalidVersionFails : true
        ]
    ]

#### bootstrapFramework.fontAwesome.install

If ```bootstrapFramework.fontAwesome.install``` is set to ```true```, the plugin will install the Font Awesome fonts using the default plugin version without LESS support.

    bootstrapFramework = [
        fontAwesome : [
            install : true
        ]
    ]

#### bootstrapFramework.fontAwesome.version

You can change the Font Awesome version by setting the ```bootstrapFramework.fontAwesome.version``` property.

    bootstrapFramework = [
        fontAwesome : [
            install : true
            version : "4.2.0"
        ]  
    ]

#### bootstrapFramework.fontAwesome.useLess

You can add LESS support for Font Awesome by setting the ```bootstrapFramework.fontAwesome.useLess``` property.

    bootstrapFramework = [
        fontAwesome : [
            install : true
            useLess : true
        ]  
    ]

### How the Plugin Works

The plugin downloads the appropriate Bootstrap or Font Awesome zip file and copies it to your application's ```build/tmp``` directory. The plugin will extract the necessary files and copy them to the directories defined by the ```bootstrapFramework.cssPath``` and ```bootstrapFramework.jsPath``` properties.

The files are copied into the directory trees shown below. It is important that you do not put any files in the two ```bootstrap``` directories (```"${bootstrapFramework.cssPath}/bootstrap"``` and ```"${bootstrapFramework.jsPath}/bootstrap"```) or the ```font-awesome``` directory (```"${bootstrapFramework.cssPath}/font-awesome"```) because they will be overwritten.

The ```bootstrap-all.js```, ```bootstrap-all.css```, ```bootstrap-less.less```, ```font-awesome-all.css```, and ```font-awesome-less.less``` files are generated for the asset-pipeline plugin if you are using Grails or the word "assets" is contained in the ```bootstrapFramework.jsPath``` property.

Directory ```bootstrapFramework.jsPath```:

    |    bootstrap-all.js
    |----bootstrap/
    |    |    affix.js
    |    |    alert.js
    |    |    bootstrap.js
    |    |    etc.

Directory ```bootstrapFramework.cssPath```:

    |    bootstrap-all.css
    |    bootstrap-less.less
    |----bootstrap/
    |    |----css/
    |    |    |    bootstrap-theme.css
    |    |    |    bootstrap.css
    |    |----fonts/
    |    |    |    glyphicons-halflings-regular.eot
    |    |    |    etc.
    |    |----less/
    |    |    |    alerts.less
    |    |    |    badges.less
    |    |    |    etc.
    |    |    |----mixins/
    |    |    |    |    alerts.less
    |    |    |    |    background-variant.less
    |    |    |    |    etc.
    |    font-awesome-all.css
    |    font-awesome-less.less
    |----font-awesome/
    |    |----css/
    |    |    |    font-awesome.css
    |    |----fonts/
    |    |    |    FontAwesome.otf
    |    |    |    etc.
    |    |----less/
    |    |    |    animated.less
    |    |    |    etc.

### User Task

You can use the task shown below to display the default Bootstrap Framework and Font Awesome versions used by the plugin.

    ./gradlew bootstrapFrameworkVersions
    
or
    
    ./gradlew bFV
    
The output will be similar to:

    3.3.5 is the default Bootstrap Framework version.
    4.3.0 is the default Font Awesome version.
   
### Glyphicons

The Glyphicons icons are available as described in the [Bootstrap Components](http://getbootstrap.com/components/) section of the Bootstrap Framework documentation.

### Asset Pipeline Usage

The remaining sections outline how to include Bootstrap Framework and Font Awesome in your application using the ```asset-pipeline-core``` plugin and its ```less-asset-pipeline``` module. 

#### JavaScript

The instructions below assume the manifest file is in the ```grails-app/assets/javascripts``` directory.

##### Add all Bootstrap JavaScript Files

Add the line below to a manifest:

    //= require bootstrap-all
  
Or add the line below to a GSP:

    <asset:javascript src="bootstrap-all.js"/>

##### Add individual Bootstrap JavaScript files

Ensure you set the parameter below to true as described [above](https://github.com/kensiprell/bootstrap-framework#bootstrapframeworkuseindividualjs):

    bootstrapFrameworkUseIndividualJs = true

Add a line similar to the one below to a manifest:

    //= require bootstrap/bootstrap-affix
  
Or add the line below to a GSP:

    <asset:javascript src="bootstrap/bootstrap-affix.js"/>

#### Stylesheets

The instructions below assume the manifest file is in the ```grails-app/assets/stylesheets``` directory.

##### Add all Bootstrap and Font Awesome CSS Files

Add the lines below to a manifest:

    *= require bootstrap-all
    *= require font-awesome-all
  
Or add the lines below to a GSP:

    <asset:stylesheet src="bootstrap-all.css"/>
    <asset:stylesheet src="font-awesome-all.css"/>

##### Add individual Bootstrap CSS Files

Add the line below to a manifest:

    *= require bootstrap/css/bootstrap-theme
  
Or add the line below to a GSP:

    <asset:stylesheet src="bootstrap/css/bootstrap-theme.css"/>

### LESS

#### Add LESS Files

Add the lines below to a manifest:

    *= require bootstrap-less
    *= require font-awesome-less
  
Or add the line below to a GSP:

    <asset:stylesheet src="bootstrap-less.css"/>
    <asset:stylesheet src="font-awesome-less.css"/>

#### LESS Customizations

If LESS support is configured for either Bootstrap Framework or Font Awesome, the plugin will create the appropriate LESS file in your application's ```bootstrapFramework.cssPath``` directory. If you later decide not to use LESS, the plugin will delete the LESS and mixin files, but it will not delete the ```bootstrap-less.less``` or the ```font-awesome-less.less``` file. Should you decide to turn LESS support back on, your customized LESS files will still be available.

Use ```bootstrap-less.less``` for customizing the Bootstrap Framework:

    /*
    * This file is for your Bootstrap Framework less and mixin customizations.
    * It was created by the bootstrap-framework plugin.
    * It will not be overwritten.
    *
    * You can import all less and mixin files as shown below,
    * or you can import them individually.
    * See https://github.com/kensiprell/bootstrap-framework/blob/master/README.md#less
    */

    @import "bootstrap/less/bootstrap.less";

    /*
    * Your customizations go below this section.
    */

Use ```font-awesome-less.less``` for customizing Font Awesome:

    * Font Awesome by Dave Gandy - http://fontawesome.io
    *
    * This file is for your Font Awesome less and mixin customizations.
    * It was created by the bootstrap-framework plugin.
    * It will not be overwritten.
    *
    * You can import all less and mixin files as shown below,
    * or you can import them individually.
    * See https://github.com/kensiprell/bootstrap-framework/blob/master/README.md#font-awesome-less
    */

    @import "font-awesome/less/font-awesome.less";

    @fa-font-path: "/assets/font-awesome/fonts";

    /*
    * Your customizations go below this section.
    */
