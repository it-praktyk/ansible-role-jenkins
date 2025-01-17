# Ansible Role: Jenkins

[![Build Status](https://travis-ci.org/it-praktyk/ansible-role-jenkins.svg?branch=master)](https://travis-ci.org/it-praktyk/ansible-role-jenkins)

Installs Jenkins server (formerly the [Jenkins](https://jenkins.io/)) on RHEL/CentOS and Debian/Ubuntu servers.

The role is forked from the role authored initially by [Jeff Geerling](https://github.com/geerlingguy). The intention for that fork is to create the role with a strong community focus role to install/manage Jenkins.

The fork base

- date: 2019-07-03
- the role version: 3.7.0
- the commit SHA: 14f0e2155fdb0896878fcfd6b269d6dbc4406fed

## Requirements

- An installed version of Java 8 or Java 11 [(except very old versions of Jenkins those work with Java 7)](https://jenkins.io/doc/administration/requirements/java/). In opposite to the [geerlingguy.java](https://galaxy.ansible.com/geerlingguy/java) role, that role doesn't install Java. For Molecule-based tests, the role [robertdebock.java](https://galaxy.ansible.com/robertdebock/java) is used.
- [`curl`](https://curl.haxx.se/) installed on the server

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    jenkins_release_line: lts

The channel used to gather `jenkins` package. Available options: `lts` and `weekly`.
A detailed explanation of LTS release line you can read [at the Jenkins webpage](https://jenkins.io/download/lts/).

    register_repository: yes

When set to `yes` the official Jenkins repository of distribution-specific packages (deb/rpm) will be registered at the system.

    jenkins_package_state: present

The state of the `jenkins` package install. By default, this role installs Jenkins but will not upgrade Jenkins (when using package-based installs). If you want to always update to the latest version, change this to `latest`.

    jenkins_hostname: localhost

The system hostname; usually `localhost` works fine. This will be used during setup to communicate with the running Jenkins instance via HTTP requests.

    jenkins_home: /var/lib/jenkins

The Jenkins home directory which, amongst others, is being used for storing artifacts, workspaces, and plugins. This variable allows you to override the default `/var/lib/jenkins` location.

    jenkins_http_port: 8080

The HTTP port for Jenkins' web interface.

    jenkins_admin_username: admin
    jenkins_admin_password: admin

Default admin account credentials which will be created the first time Jenkins is installed.

    jenkins_jar_location: /opt/jenkins-cli.jar

The location at which the `jenkins-cli.jar` jar file will be kept. This is used for communicating with Jenkins via the CLI.

    jenkins_plugins: []

Jenkins plugins to be installed automatically during provisioning.

    jenkins_plugins_install_dependencies: true

Whether Jenkins plugins to be installed should also install any plugin dependencies.

    jenkins_plugins_state: present

Use `latest` to ensure all plugins are running the most up-to-date version.

    jenkins_plugin_updates_expiration: 86400

Number of seconds after which a new copy of the update-center.json file is downloaded. Set it to 0 if no cache file should be used.

    jenkins_updates_url: "https://updates.jenkins.io"

The URL to use for Jenkins plugin updates and update-center information.

    jenkins_plugin_timeout: 30

The server connection timeout, in seconds, when installing Jenkins plugins.

    jenkins_version: "1.644"

Then Jenkins version can be pinned to any version available for Jenkins release lines
    - `https://pkg.jenkins.io/debian-stable/` (Debian/Ubuntu - lts)
    - `https://pkg.jenkins.io/redhat-stable/` (RHEL/CentOS - lts)
    - `https://pkg.jenkins.io/debian/` (Debian/Ubuntu - weekly)
    - `https://pkg.jenkins.io/redhat/` (RHEL/CentOS - weekly)

    jenkins_pkg_url: "http://www.example.com"

If the Jenkins version you need is not available in the default package URLs, you can override the URL with your own; set `jenkins_pkg_url` (_Note_: the role depends on the same folder structure and packages naming convention that `https://pkg.jenkins.io/` uses).

    jenkins_predownloaded_package_path: ""

The path to the Jenkins package rpm/deb. If the variable is set the file has to be delivered to a managed node before running the role and a Jenkins version is not verified.

    jenkins_url_prefix: ""

Used for setting a URL prefix for your Jenkins installation. The option is added as `--prefix={{ jenkins_url_prefix }}` to the Jenkins initialization `java` invocation, so you can access the installation at a path like `http://www.example.com{{ jenkins_url_prefix }}`. Make sure you start the prefix with a `/` (e.g. `/jenkins`).

    jenkins_connection_delay: 5
    jenkins_connection_retries: 60

Amount of time and number of times to wait when connecting to Jenkins after initial startup, to verify that Jenkins is running. Total time to wait = `delay` * `retries`, so by default, this role will wait up to 300 seconds before timing out.

    # For RedHat/CentOS (role default):
    jenkins_repo_url: http://pkg.jenkins.io/redhat/jenkins.repo
    jenkins_repo_key_url: http://pkg.jenkins.io/redhat/jenkins.io.key
    # For Debian (role default):
    jenkins_repo_url: deb http://pkg.jenkins.io/debian binary/
    jenkins_repo_key_url: http://pkg.jenkins.io/debian/jenkins.io.key

This role will install the latest version of Jenkins by default (using the official repositories as listed above). You can override these variables (use the correct set for your platform) to install the current LTS version instead:

    # For RedHat/CentOS LTS:
    jenkins_repo_url: http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
    jenkins_repo_key_url: http://pkg.jenkins-ci.org/redhat-stable/jenkins-ci.org.key
    # For Debian/Ubuntu LTS:
    jenkins_repo_url: deb http://pkg.jenkins-ci.org/debian-stable binary/
    jenkins_repo_key_url: http://pkg.jenkins-ci.org/debian-stable/jenkins-ci.org.key

It is also possible stop registering the repository (the file is added always) by using setting the variable `register_repository`.

    jenkins_java_options: "-Djenkins.install.runSetupWizard=false"

Extra Java options for the Jenkins launch command configured in the init file can be set with the var `jenkins_java_options`. For example, if you want to configure the timezone Jenkins uses, add `-Dorg.apache.commons.jelly.tags.fmt.timeZone=America/New_York`. By default, the option to disable the Jenkins 2.0 setup wizard is added.

    jenkins_init_changes:
      - option: "JENKINS_ARGS"
        value: "--prefix={{ jenkins_url_prefix }}"
      - option: "JENKINS_JAVA_OPTIONS"
        value: "{{ jenkins_java_options }}"

Changes made to the Jenkins init script; the default set of changes set the configured URL prefix and add in configured Java options for Jenkins' startup. You can add other option/value pairs if you need to set other options for the Jenkins init file.

## Dependencies

- none

## Example Playbook

```yaml
- hosts: jenkins
  vars:
    jenkins_hostname: jenkins.example.com
  roles:
    - role: it-praktyk.jenkins
      become: yes
```

## License

MIT (Expat) / BSD

## Author Information

The current maintainer/author: Wojciech Sciesinski wojciech[at]sciesinski[dot]net.

This role was initially created in 2014 by [Jeff Geerling](https://github.com/geerlingguy).
