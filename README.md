galaxyproject.opendkim
======================

Install and configure [OpenDKIM][opendkim] on Debian- and RedHat-based systems.

[opendkim]: http://opendkim.org/

Requirements
------------

None

Role Variables
--------------

Most variables are self-explanatory, see the `opendkim_*` variables in the [defaults][defaults] file for a list. A few
are explained here:

- `opendkim_keys`: List of keys to generate or install. Each list item is a hash with keys `domain`, `selector`, and
  `bits` corresponding to the flags to `opendkim-genkey`, as well as `secret` and `public`. If `secret` and `public` are
  not specified, the key will be generated. If they are specified, the other keys are not required (although at least
  `selector` is probably necessary to specify the key filenames). 

[defaults]: defaults/main.yml

Dependencies
------------

None, but the [galaxyproject.postfix][postfix-role] role can be used in conjunction with this role to configure Postfix
for DKIM mail.

[postfix-role]: https://github.com/galaxyproject/ansible-postfix/

Example Playbook
----------------

```yaml
- hosts: all
  vars:
    opendkim_keys:
      # generate a new key
      - selector: mail
        domain: example.org
        bits: 2048
      # provide an existing key
      - selector: lists
        domain: lists.example.org
        secret: |
          -----BEGIN RSA PRIVATE KEY-----
          MII...
          -----END RSA PRIVATE KEY-----
        public: "v=DKIM1; h=sha256; k=rsa; p=MII..."
  roles:
    - galaxyproject.opendkim
```

License
-------

MIT

Author Information
------------------

- [Nate Coraor](https://github.com/natefoo)
