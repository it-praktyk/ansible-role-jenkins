# Changelog

## Next version (Unreleased)

BREAKING CHANGES:

FEATURES:

IMPROVEMENTS:

BUG FIXES:

## 4.0.1 - 2019-09-01

BREAKING CHANGES:

- Switched compatibility to Ansible >= 2.7
- Corrected list of compatible OS'es

BUG FIXES:

- Correction of Ansible Galaxy metadata

## 4.0.0 - 2019-09-01

BREAKING CHANGES:

- The default Jenkins release line changes from the `weekly` to the `lts` - [GH-1]
- Java will not be installed by the role. You have installed Java separatelly due that a dependency to geerlingguy.java was removed - [GH-4]
- The variable `jenkins_admin_password_file` was removed - [GH-7]

FEATURES:

- Add option to select Jenkins release line [GH-1]
- Add option to skip registering Jenkins repository [GH-6]
- Add option to install pre-downloaded Jenkins package [GH-7]

IMPROVEMENTS:

- The CHANGELOG file added [GH-2]
- Make installation of a certain version of package idenpontent [GH-7]
- Improve Molecule-based tests [GH-7]
