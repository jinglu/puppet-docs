---
layout: default
title: "Directories: The Modulepath"
---




Puppet loads most of its content from [modules][].




























The **modulepath** is a list of one or more directories that Puppet will search for [modules][] in.

**This usually includes `$confdir/modules`,** which is the de-facto "main" modulepath. ([See here for info about the confdir][confdir].)

If you are using [environments][], each environment usually has a different modulepath.


## Default Locations

* By default, the modulepath will use the value of the `basemodulepath` setting. Its default value depends on OS and Puppet distribution; see the table below.
* If there's an [environment directory][env_dirs] that matches the current node's environment, Puppet will put its [module directory][env_module_dir] in front of the base modulepath.

### Default Base Modulepaths

This table lists the default `basemodulepath` values. Note that all default values include `$confdir/modules`; [see here for info about the confdir][confdir].

OS and Distro             | Default Base Modulepath
--------------------------|----------------------------------------------------
\*nix (Puppet Enterprise) | `$confdir/modules:/opt/puppet/share/puppet/modules`
\*nix (open source)       | `$confdir/modules:/usr/share/puppet/modules`
Windows (PE and foss)     | `$confdir\modules`

### Examples

On an open source Linux puppet master serving an agent in the `dev` environment (using [environment directories][env_dirs] instead of [config file environments][env_conf]), the default modulepath would be:

`/etc/puppet/environments/dev/modules:/etc/puppet/modules:/usr/share/puppet/modules`

## Multi-Directory Paths

The modulepath often includes multiple directories. When it does, the directories are separated by the system path-separator character. On \*nix systems this is the colon (:), and on Windows it is the semi-colon (;).

For example:

`/etc/puppet/environments/production/modules:/etc/puppet/modules:/usr/share/puppet/modules`

### Overriding Modules

If multiple directories have a module with the same name, the one closest to the front of the modulepath will be used.

For example: in the modulepath above, if both `/etc/puppet/environments/production/modules` and `/etc/puppet/modules` have a module named `ntp`, the version in `/etc/puppet/environments/production/modules` will be used.

This allows environments to override modules from the "main" modulepath with newer versions, so that changes can be tested on a small number of nodes.

> **Note:** Although Puppet tries to override modules cleanly, there are sometimes problems with custom resource types and custom functions in modules. This is because the way a puppet master process loads Ruby plugins prevents it from distinguishing between different versions of them. Although agent nodes will properly download the versions of plugins they need, the master may use the wrong version when compiling their catalogs.
>
> This issue is being tracked as [PUP-731](https://tickets.puppetlabs.com/browse/PUP-731).




## Configuring the Modulepath

The modulepath can be configured with the [`modulepath`][modulepath] and [`basemodulepath`][basemodulepath] settings. Additionally, a portion of the modulepath may be automatically configured by an [environment directory][env_dirs]. To check the actual modulepath on one of your nodes, [use the puppet config command][print_settings] to see the value of the `modulepath` setting.

By default, the `ssldir` is located at `$confdir/ssl`. ([See here for info about the confdir][confdir].)


Generally, the main directory in the modulepath is `$confdir/modules`. If you are using [environments][], the environment's modules directory usually overrides the default modulepath.
The modulepath can be one directory or several directories, listed in order of precedence. The directories should be separated by the system path-separator character --- on \*nix systems this is the colon (:), and on Windows it is the semi-colon (;).

### Central vs. Environmental Modulepaths

### Modulepath Configuration




This list of directories can be configured with the [`modulepath` setting][modulepath], where they should

The list can also be influenced by the [`basemodulepath` setting][basemodulepath].

By default, the  is located at `$confdir/modules`.


## Contents

Every item in the modulepath should be a valid Puppet module. For details about module contents and structure, see [the documentation on modules][modules].

## Behavior When Loading Modules

Items earlier in the modulepath will override items later in the modulepath.

TODO: verify how this works -- by module? By individual file? Do plugins and manifests and templates all work the same?











> ### The Modulepath
>
> **Note:** The `modulepath` is a list of directories separated by the system path-separator character. On 'nix systems, this is the colon (:), while Windows uses the semi-colon (;). The most common default modulepaths are:
>
> * `/etc/puppetlabs/puppet/modules:/opt/puppet/share/puppet/modules` (for Puppet Enterprise)
> * `/etc/puppet/modules:/usr/share/puppet/modules` (for open source Puppet)
>
> Use `puppet config print modulepath` to see your currently configured modulepath.
>
>  If you want both puppet master and puppet apply to have access to the modules, set the modulepath in [puppet.conf][conf] to go to the `[main]` section. Modulepath is also one of the settings that can be different per [environment][].





















