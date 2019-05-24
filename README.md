ansible-php-role
=========

Ansible role to manage PHP FPM for Debian. Supports multiple pool configurations

Requirements
------------

None

Role Variables
--------------
```YAML
#
# PHP Configuration
#
# Most of the default values (specially php.ini) are the default values provided
#

# Install and enable php fpm
php_fpm_enabled: true

# PHP version to install
php_version: 7.3

# Run php fpm as daemon
php_fpm_daemonize: "yes"

# Existing webserver is in use
php_webserver_enabled: false

# Webserver service
php_webserver_service: nginx

#
# PHP packages and repository configuration
#

# Latest will install and update to latest versions, present will not update
php_packages_mode: latest # present|latest

# Add and use the sury repository (Debian only)
php_sury_repository_enabled: true

# Install additional php packages
php_extra_packages: []

#
# Error log configuration
#

# Path of the error log
php_fpm_error_log: /var/log/php{{ php_version }}-fpm.log

# Error log level
php_fpm_log_level: notice

#
# PHP FPM pool configuration
#

# Removal of the defalt pool
php_fpm_remove_default_pool: true

# List of php fpm pools to be added with an example pool item
php_fpm_pools:
  # Pool configuration
  - name: poolname
    filename: tld.pool.conf
    user: user
    group: group
    
    # Listen configuration
    listen:
      address: "/run/php/php-{{ php_version }}-fpm-poolname.sock"
      backlog: 511
      owner: "{{ php_user_group }}"
      group: "{{ php_user_group }}"
      mode: "0660"
      allowed_clients: 127.0.0.1
      
    # Process manager configuration
    pm:
      mode: dynamic
      max_children: 10
      start_servers: 3 # min_spare_servers + (max_spare_servers - min_spare_servers) / 2
      min_spare_servers: 1
      max_spare_servers: 3
      process_idle_timeout: 10s
      max_requests: 0
      
    # Environment variables
    env_vars: |
      ;env[HOSTNAME] = $HOSTNAME
      ;env[PATH] = /usr/local/bin:/usr/bin:/bin
      ;env[TMP] = /tmp
      ;env[TMPDIR] = /tmp
      ;env[TEMP] = /tmp
      :env[APPLICATION_ENV] = production

#
# PHP ini basic settings
#
# NOTE: If you set "php_manage_ini_file" to false, all of the below settings are ignored
#

# Disable managed php.ini to configure it on your own. All ansible php.ini variables are ignored if set to false
php_manage_ini_file: true

# php.ini settings only used when php_manage_ini_file is set to true
php_short_open_tags: "Off"
php_output_buffering: 4096
php_disable_functions:
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
php_disable_classes: []
php_realpath_cache_size: 4096k
php_realpath_cache_ttl: 120
php_expose_php: "Off"
php_max_execution_time: 30
php_max_input_time: 60
php_memory_limit: -1
php_error_reporting: E_ALL & ~E_DEPRECATED & ~E_STRICT
php_display_errors: "Off"
php_display_startup_errors: "Off"
php_log_errors: "On"
php_post_max_size: 8M
php_default_charset: UTF-8
php_file_uploads: "On"
php_upload_max_filesize: 2M
php_max_file_uploads: 20
php_allow_url_fopen: "On"
php_allow_url_include: "Off"
php_date_timezone: Europe/Berlin

#
# PHP ini session settings
#
php_session_save_handler: files
php_session_save_path:
php_session_name: PHPSESSID
php_session_cookie_lifetime: 0

#
# PHP ini opcache settings
#
php_opcache_enable: 1
php_opcache_enable_cli: 0
php_opcache_memory_consumption: 128
php_opcache_max_accelerated_files: 10000
php_opcache_max_wasted_percentage: 5
php_opcache_use_cwd: 1
php_opcache_validate_timestamps: 1
php_opcache_revalidate_freq: 2
php_opcache_revalidate_path: 0
php_opcache_enable_file_override: 0
php_opcache_max_file_size: 0
php_opcache_consistency_checks: 0
php_opcache_force_restart_timeout: 180
php_opcache_error_log:
php_opcache_log_verbosity_level: 1
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
