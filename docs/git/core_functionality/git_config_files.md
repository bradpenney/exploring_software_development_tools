# Configuration Files for Git

The configuration files for Git are stored in three different locations:

1. **[Optional] System-Wide**: located in `/etc/gitconfig`, settings stored here affect all users
   of a system. It is very common for this this file to be empty or non-existent, as
   shared computers are relatively rare.
1. **User-Specific**: stored in `~/.gitconfig`, the per-user configuration file for `git`.  This stores information
   about the specific user, such as their name and email address.
1. **Project-Specific**: stored in `.git/config`, this is the configuration file for a specific project.

The order of precedence for these files is the same as they are listed above. In other words, project-specific
configurations override user-specific configurations, which in turn override system-wide configurations.
