This guide is intended for people who have never used a permissions plugin before. If you're familiar with the concept, and think you'll be able to understand things just from reading the command usages, I suggest you read the page on [Command Usage and Permissions](https://github.com/lucko/LuckPerms/wiki/Command-Usage), as this gives a much more "straight to the point" description of how the plugin works.

If you're struggling to understand that, then this guide is a good place to start. :)


# Key Definitions
### Permission
 On your server, there will be a number of **features, commands, and functionality** which is added to the game. Some of these features will be included with the server, and others are added with “plugins”. Most of these actions **have a permission associated with them**, so you can control which users have access to each feature or command.
 
A **permission is just a string**, and is separated into parts using periods. For example, “minecraft.command.ban” is the permission for the /ban command. Obviously we don’t want all users to have access to this, so we only give it to users we trust. 

The string that represents a certain permission is also sometimes called a “permission node” or just “node” for short.

### Group
Instead of assigning permissions to every user individually, we have **groups of permissions**, which can then be **assigned to a user** as a whole.

For example, in my “admin” group, I might add permission to use the ban and unban commands, and then assign users to the admin group. This means that they will get all of the permissions from “admin”, plus any they have themselves.

### Inheritance
Users and groups are able to **inherit permissions from each other**. For example, by default, all users inherit permissions from the “default” group. You can setup your own groups an inheritances for your server, and make your own unique system.

For example, I might have 3 groups, “default”, “moderator” and “admin”. I want moderator to inherit permissions from default, and admin to inherit permissions from moderator.


# Getting Started
If you haven't got LuckPerms installed just yet, please refer to the [installation guide](https://github.com/lucko/LuckPerms/wiki/Setup) first.

Then, please make sure you read the section about [Choosing a Storage type](https://github.com/lucko/LuckPerms/wiki/Choosing-a-Storage-type) before proceeding. Whilst it is possible to change these options later, it's better to get them right the first time around.

## Granting full access
The first thing you'll want to do is give yourself full access to the server.

To do this, we're just going to give our user access to everything. 

You need to type  `/luckperms user Luck permission set luckperms.* true` into your server console, of course, replacing my username with your own. (don't worry, the usage of this command will be explained later)

This should be the result:    
![](http://i.imgur.com/zaw4l7q.png)

Effectively, what this command does, is give the user `Luck` the `luckperms.*` permission. (or sets it to true for the user)

> You'll notice we just used the `*` character at the end of the permission string. This character is called a wildcard, and gives a user access to **all** permissions which start with "luckperms".

## Creating the first group
We can create a new group with the create group command command.

Let's create a new group called admin, and then give it a permission.

First, run [`/luckperms creategroup admin`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#lp-creategroup). This will create a new empty group named "admin".

![](http://i.imgur.com/3mz08n1.png)

Next, we want to add a permission to the admin group. The command to modify a group is [`/luckperms group <group>`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#group---lp-group-group-). If you run the command, it will list each of the subcommands back to you.

![](http://i.imgur.com/CPiZK5G.png)

You'll notice that the first command is the "info" command. This just lists some information about the group.

We can run [`/luckperms group admin info`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#lp-group-group-info) to see some information about the new admin group.

![](http://i.imgur.com/agliG4f.png)

The next command down is the "permission" command. This allows you to modify the permissions held by the group. Again, running [`/luckperms group admin permission`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#permission---lp-user-user-permission---lp-group-group-permission-) will list the available sub-commands.

![](http://i.imgur.com/T4P5YFy.png)

Again, we see more commands we can use. The first is another "info" command. Since it's a sub command of "permission", this info command returns information about the permissions a group has. The next command however is the "set" command.

Remember, we used this earlier to give a user access to the "luckperms.*" permission. It works the same here.

Just running the command without any arguments will return information about how to use it. For example:    
![](http://i.imgur.com/8h16DV0.png)

For example, I want to give my admin group access to "minecraft.command.ban". I can therefore just run [`/luckperms group admin permission set minecraft.command.ban true`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#lp-usergroup-usergroup-permission-set).

![](http://i.imgur.com/McXI5Nx.png)

This command is giving `admin` access to the `minecraft.command.ban` permission. The true at the end is the value we're assigning the permission as. You can either set a permission to `true` or `false`. Setting a permission as true gives the user or group access to it, and setting it to false negates it. (specifically doesn't give them access)

If I decide later that I don't want admin to have this permission anymore, I can just use the unset command to remove it, with [`/luckperms group admin permission unset minecraft.command.ban`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#lp-usergroup-usergroup-permission-unset).

![](http://i.imgur.com/x1ecIQo.png)

## Adding a user to a group
Adding users to a group can be done with the "parent" command. (we just swap "permission" for "parent" in our command usage)

For example, to add myself to the admin group, I would run [`/luckperms user Luck parent add admin`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#lp-usergroup-usergroup-parent-add).

![](http://i.imgur.com/eScw7gC.png)

This command adds the user `Luck` to the `admin` group. This means that any permissions admin has, I also have through inheritance.

## Making a group inherit another group
As well as users, groups are also able to inherit other groups.

For example, suppose the following setup. (some of the permissions are just made up)

| Admin | Mod | Default |
|-------|-----|---------|
| minecraft.command.ban | minecraft.command.mute | minecraft.command.say |
| minecraft.command.pardon | minecraft.command.unmute | minecraft.command.me |
| some.cool.admin.perm | some.cool.mod.perm | |
| someplugin.vanish | chatcolor.bold | |

I want users in my admin group to also have access to mod and default permissions, and I want users in the mod group to have access to default's permissions.

To achieve this, I can setup the groups to inherit from each other.

The command [`/luckperms group admin parent add mod`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#lp-usergroup-usergroup-parent-add) will make admin inherit all of mods permissions. I can then do the same for mod, and run `/luckperms group mod parent add default`.

![](http://i.imgur.com/tYcKGe6.png)

The inheritance is recursive, so since although admin how only inherits directly from mod, mod inherits from default. This means admin has access to both the permissions in mod **and** default.

A user in admin has access therefore to `minecraft.command.ban` and `minecraft.command.mute`, *and* `minecraft.command.say`.

## Removing parent groups
Removing parent groups is done with a spookily similar command.

To remove myself from admin, I'd just run [`/luckperms user Luck parent remove admin`](https://github.com/lucko/LuckPerms/wiki/Command-Usage#lp-usergroup-usergroup-parent-remove)

![](http://i.imgur.com/Fa4Mlgs.png)