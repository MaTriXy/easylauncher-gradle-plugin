# Easylauncher gradle plugin for Android

Modify the launcher icon of each of your app-variants using simple gradle rules. Add ribbons of any color, overlay your own images, change the colors of the icon, ...

![](icons/ic_launcher_debug.png) ![](icons/ic_launcher_custom.png) ![](icons/ic_launcher_beta.png) ![](icons/ic_launcher_grayscale.png)

## Usage

```groovy
// in build.gradle
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.2'
        classpath 'com.akaita.android:easylauncher:1.0.0'
    }
}
```

```groovy
// in app/build.gradle
apply plugin: 'com.akaita.android.easylauncher'
    
android {
    buildTypes {
        debug {
            //Debuggable, will get a default ribbon in the launcher icon
        }
        beta {
            //Debuggable, will get a default ribbon in the launcher icon
            debuggable true
        }
        canary {
            //Non-debuggable, will not get any default ribbon
            debuggable false
        }
        release {
            //Non-debuggable, will not get any default ribbon
        }
    }
    productFlavors {
        local {}
        qa {}
        staging {}
        production {}
    }
}
```


Optionally, customize the plugin's behaviour
```groovy
easylauncher {
    // "manifest application[android:icon]" is automatically added to the list
    iconNames "@mipmap/ic_launcher_foreground" // Traditional launcher icon
    foregroundIconNames "@mipmap/ic_launcher_foreground" // Foreground of adaptive launcher icon
    
    productFlavors {
        local {}
        qa {
            // Add one more filter to all `qa` variants
            filters = redRibbonFilter()
        }
        staging {}
        production {}
    }
    
    buildTypes {
        beta {
            // Add two more filters to all `beta` variants
            filters = [
                    customColorRibbonFilter("#0000FF"),
                    overlayFilter(new File("example-custom/launcherOverlay/beta.png"))
            ]
        }
        canary {
            // Remove ALL filters to `canary` variants
            enable false
        }
        release {}
    }
    
    variants {
        productionDebug {
            // OVERRIDE all previous filters defined for `productionDebug` variant
            filters = orangeRibbonFilter()
        }
    }
}
```


## Available filters

| Filter | Result |
| - | - |
| `grayRibbonFilter()` | ![](icons/grayRibbon.png) |
| `greenRibbonFilter()` | ![](icons/greenRibbon.png) |
| `yellowRibbonFilter()` | ![](icons/yellowRibbon.png) |
| `orangeRibbonFilter()` | ![](icons/orangeRibbon.png) |
| `redRibbonFilter()` | ![](icons/redRibbon.png) |
| `blueRibbonFilter()` | ![](icons/blueRibbon.png) |
| `grayscaleFilter()` | ![](icons/grayscale.png) |
| `customColorRibbonFilter("#6600CC")` | ![](icons/customColorRibbon.png) |
| `overlayFilter(new File("example-custom/launcherOverlay/beta.png"))` | ![](icons/overlay.png) |



## Project Structure

```
plugin/          - Gradle plugin
example-simple/  - Example Android application using easylauncher with the default behaviour
example-custom/  - Example Android application using easylauncher with the custom configuration
buildSrc/        - Helper module to use this plugin in example modules
icons/           - Examples of icons generated by this plugin
```


## Contributing

You can already see my plans for the plugin in the project's Issues section.  

Still, I'm open to feature-requests and suggestions.  
Of course, a PR is the best way to get something you want into the plugin ;)


## Credits

Easylauncher started as an extension of [Fuji Goro's `ribbonizer` plugin](https://github.com/maskarade/gradle-android-ribbonizer-plugin). 
As it evolved, I decided it changed enough to be worth releasing as a separate plugin.
