Lighthouse
==========

This role installs Lighthouse on EL

Role Variables
--------------

|vars| description|
|------|------------|
|lighthouse_vcs| lighthouse version to install|

Dependencies
------------

Lighthouse depends on NGINX service. It should be installed prior.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: lighthouse }

License
-------

MIT

Author Information
------------------

Constantine Chernevsky
