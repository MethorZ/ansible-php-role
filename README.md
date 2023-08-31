ansible-php-role
=========

Ansible role to manage PHP FPM for Debian. Supports multiple pool configurations

Requirements
------------

None

Role Variables
--------------
```YAML
---
##
# PHP default configuration
##

# Install and enable php fpm
php_fpm_enabled: false

# PHP version to install
php_fpm_version: 8.2

# Install php as cli only (no additional setup required)
php_fpm_cli_only: false

# Run php fpm as daemon
php_fpm_daemonize: "yes"

# Existing webserver is in use
php_fpm_webserver_enabled: false

# Webserver service
php_fpm_webserver_service: nginx

#
# PHP packages and repository configuration
#

# Latest will install and update to latest versions, present will not update
php_fpm_packages_mode: present # present|latest

# Add and use the sury repository (Debian only)
php_fpm_sury_repository_enabled: true

# Install additional php packages
php_fpm_packages_extra: []

#
# Error log configuration
#

# Path of the error log
php_fpm_error_log: /var/log/php{{ php_fpm_version }}-fpm.log

# Error log level
php_fpm_log_level: notice

#
# PHP FPM pool configuration
#

# Removal of the default pool
php_fpm_remove_default_pool: true

#
# PHP FPM - Pools configuration
#
php_fpm_pools:
  #
  # Example - PHP FPM POOL
  #
  - name: example
    filename: tld.example
    user: example
    group: example

    # Listen configuration
    listen:
      address: "/run/php/php8.2-fpm-example.sock"
      backlog: 511
      owner: "{{ php_fpm_user_group }}"
      group: "{{ php_fpm_user_group }}"
      mode: "0660"
      allowed_clients: 127.0.0.1

    # Process manager configuration
    pm:
      mode: dynamic
      max_children: 100
      start_servers: 20 # min_spare_servers + (max_spare_servers - min_spare_servers) / 2
      min_spare_servers: 10
      max_spare_servers: 30
      process_idle_timeout: 10s
      max_requests: 0
      status_path: "/example-fpm-status"

    # Environment variables
    env_vars: |
      env[APPLICATION_ENV] = production

#
# PHP ini basic settings
#
# NOTE: If you set "php_fpm_manage_ini_file" to false, all the below settings are ignored
#

# Disable managed php.ini to configure it on your own. All ansible php.ini variables are ignored if set to false
php_fpm_manage_ini_file: true

# php.ini settings only used when php_fpm_manage_ini_file is set to true
php_fpm_short_open_tags: "Off"
php_fpm_output_buffering: 4096
php_fpm_disable_functions:
  - pcntl_alarm
  - pcntl_fork
  - pcntl_waitpid
  - pcntl_wait
  - pcntl_wifexited
  - pcntl_wifstopped
  - pcntl_wifsignaled
  - pcntl_wifcontinued
  - pcntl_wexitstatus
  - pcntl_wtermsig
  - pcntl_wstopsig
  - pcntl_signal
  - pcntl_signal_get_handler
  - pcntl_signal_dispatch
  - pcntl_get_last_error
  - pcntl_strerror
  - pcntl_sigprocmask
  - pcntl_sigwaitinfo
  - pcntl_sigtimedwait
  - pcntl_exec
  - pcntl_getpriority
  - pcntl_setpriority
  - pcntl_async_signals
php_fpm_disable_classes: []
php_fpm_realpath_cache_size: 4096k
php_fpm_realpath_cache_ttl: 120
php_fpm_expose_php: "Off"
php_fpm_max_execution_time: 30
php_fpm_max_input_time: 60
php_fpm_memory_limit: -1
php_fpm_error_reporting: E_ALL & ~E_DEPRECATED & ~E_STRICT
php_fpm_display_errors: "Off"
php_fpm_display_startup_errors: "Off"
php_fpm_log_errors: "On"
php_fpm_post_max_size: 8M
php_fpm_default_charset: UTF-8
php_fpm_file_uploads: "On"
php_fpm_upload_max_filesize: 2M
php_fpm_max_file_uploads: 20
php_fpm_allow_url_fopen: "On"
php_fpm_allow_url_include: "Off"
php_fpm_date_timezone: Europe/Berlin

#
# PHP ini session settings
#
php_fpm_session_save_handler: files
php_fpm_session_save_path: "/var/lib/php/sessions"
php_fpm_session_name: PHPSESSID
php_fpm_session_cookie_lifetime: 0

#
# PHP ini opcache settings
#
php_fpm_opcache_enable: 1
php_fpm_opcache_enable_cli: 0
php_fpm_opcache_memory_consumption: 128
php_fpm_opcache_max_accelerated_files: 10000
php_fpm_opcache_max_wasted_percentage: 5
php_fpm_opcache_use_cwd: 1
php_fpm_opcache_validate_timestamps: 1
php_fpm_opcache_revalidate_freq: 2
php_fpm_opcache_revalidate_path: 0
php_fpm_opcache_enable_file_override: 0
php_fpm_opcache_max_file_size: 0
php_fpm_opcache_consistency_checks: 0
php_fpm_opcache_force_restart_timeout: 180
php_fpm_opcache_error_log:
php_fpm_opcache_log_verbosity_level: 1

#
# PHP extensions
#
php_fpm_enabled_extensions: []

php_fpm_pecl_extensions: []

```

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: methorz.php }

License
-------

BSD

Author Information
------------------

https://twitter.com/methor_z
