NOTE: Not finished

# JellyfinDependOn

A lightweight alternative to [JellyfinLoader](https://github.com/stenlan/JellyfinLoader/) that provides an easy way for Jellyfin plugins to depend on other plugins.

## Usage

Simply install the NuGet package, and change your `meta.json`'s `assemblies` line to `["JellyfinDependOn.dll"]`. Then include a `dependencies.json` that looks like the following:

```jsonc
{
  "assemblies": [], // can be either empty, or set to the "assemblies" value previously in your plugin's "meta.json".
  "installBehavior": "force", // either "force" or "ask"
  "dependencies": [ // currently, just 1 dependency is supported
    {
      "manifest": "https://your-dependency.com/manifest.json",
      "id": "guid-of-your-dependency-plugin",
      "version": "5.0.0" // currently, just one version is supported. Something akin to semver will be implemented in the future
    }
  ]
}
```

Make sure you ship the `JellyfinDependOn.dll` with your plugin.

## Limitations

- Because your plugin needs to load _after_ other plugins have been loaded to properly try and see if its dependency has been loaded and to inject itself into the dependency's load context, any `IPluginServiceRegistrator`s defined in your dependent assembly will not run. You can, however, include a separate assembly that does not reference your main assembly nor your dependency, in which you are free to define an `IPluginServiceRegistrator`. Note that you do not need to define a second `BasePlugin` in that assembly.
- Using JellyfinDependOn, your plugin can only have a single dependency. To have multiple dependencies, it is recommended to use [JellyfinLoader](https://github.com/stenlan/JellyfinLoader/) instead. In the future, a limited version of multiple dependency support might be added to JellyfinDependOn, where you can have multiple assemblies for the same plugin that each have their own separate single dependency.
