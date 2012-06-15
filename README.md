# Knife sharp plugin

This plugin is used to align an environment cookbooks on a given git branch. You will need the grit gem to use it too.


# Tell me more

When you want an environment to reflect a given branch you have to check by hand (or using our consistency plugin), and some mistakes can be made. This plugin aims to help to push the right version into an environment.

# Show me !

<pre>
[mordor:~] knife environment show sandboxnico
chef_type:            environment
cookbook_versions:
...
  syslog:  0.0.16
...
[mordor:~] knife sharp align syslog_double sandboxnico

Will change in environment sandboxnico :
* syslog gets version 0.0.17
Upload and set version into environment sandboxnico ? Y/N
Y
Successfull upload for syslog
Aligning 1 cookbooks
Done.
</pre>

Then we can check environment :

<pre>
[mordor:~] knife environment show sandboxnico 
chef_type:            environment
cookbook_versions:
...
  syslog:  0.0.17
...
[mordor:~] knife sharp align syslog_double sandboxnico
Nothing to do : sandboxnico has same versions as syslog_double
</pre>

It will upload the cookbooks (to ensure they meet the one on the branch you're working on) and will set the version to the required number.

# Configuration

The plugin will search in 2 places for its config file :
* "/etc/sharp-config.yml"
* "~/.chef/sharp-config.yml"

## Cookbooks path & git
If your cookbook_path is not the root of your git directory then the grit gem will produce an error. This can be circumvented by adding the following directive in your config file :

<pre>
global:
  git_cookbook_path: "/home/nico/sysadmin/chef/"
</pre>

As we version more than the cookbooks in the repo.

## Logging
It's good to have things logged. The plugin can do it for you. Add this to your config file
<pre>
logging:
  enabled: true
  destination: "~/.chef/sharp.log"
</pre>

It will log uploads, bumps and databags to the standard logger format.

# See also
The damn good knife spork plugin from the etsy folks : https://github.com/jonlives/knife-spork

License
=======
3 clauses BSD

Author
======
Nicolas Szalay < nico |at| rottenbytes |meh| info >
