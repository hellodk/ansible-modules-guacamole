# Ansible Collection - scicore.guacamole

Ansible modules to administer apache guacamole connections and users using the API

## Installation

Download the ansible collection to the root folder of your playbook:

```
$> cd /path/to/playbook/root/folder

$> ansible-galaxy collection install -p ./collections/ scicore.guacamole
```

Now you can use the modules `guacamole_connection` and `guacamole_user`. Adapt the examples below to your needs.


## Example playbook:

```


- name: Add a guacamole connection and user
  hosts: localhost

  tasks:

    - name: Add a new RDP connection
      scicore.guacamole.guacamole_connection:
        base_url: http://localhost/guacamole
        auth_username: guacadmin
        auth_password: guacadmin
        connection_name: test_connection
        protocol: rdp
        hostname: 192.168.33.43
        port: 3389
        username: rdp_user
        password: rdp_pass
      register: _connection_info

    - debug:
        var: _connection_info

    - name: Add a new guacamole user
      scicore.guacamole.guacamole_user:
        base_url: http://localhost/guacamole
        auth_username: guacadmin
        auth_password: guacadmin
        username: play_user_2
        password: aaaaaaaa
        full_name: "John"
        email: "aaaaa@hotmail.com"
        organization: "scicore"
        allowed_connections:
          - test_connection
      register: _user_info

    - debug:
        var: _user_info

```

## Output of "ansible-doc scicore.guacamole.guacamole_connection"

```
> GUACAMOLE_CONNECTION    (~/.ansible/collections/ansible_collections/scicore/guacamole/plugins/modules/guacamole_connection.py)

        Add or remove guacamole connections. You can create rdp, vnc, ssh or telnet connections

  * This module is maintained by The Ansible Community
OPTIONS (= is mandatory):

= auth_password
        Guacamole admin user password to login to the API

        type: str

= auth_username
        Guacamole admin user to login to the API

        type: str

= base_url
        Url to access the guacamole API

        type: str

= connection_name
        Name of the new connection to create

        type: str

- hostname
        Hostname or ip of the server to connect
        [Default: (null)]
        type: str

= max_connections
        Max simultaneos connections allowed for this connection

        type: int

- parentIdentifier
        Parent indentifier where to create the connection
        [Default: ROOT]
        type: str

- password
        Password for the connection
        [Default: (null)]
        type: str

- port
        Port to connect
        [Default: (null)]
        type: int

= protocol
        Protocol to use for the new connection
        (Choices: rdp, vnc, ssh, telnet)
        type: str

- sftp_default_upload_directory
        File browser default upload directory
        [Default: (null)]
        type: str

- sftp_enable
        Should we enable sftp transfers for this connection?
        [Default: (null)]
        type: bool

- sftp_hostname
        Hostname or ip for sftp
        [Default: (null)]
        type: str

- sftp_password
        Password for sftp
        [Default: (null)]
        type: str

- sftp_port
        Port to use for sftp
        [Default: (null)]
        type: int

- sftp_private_key
        Private key for sftp authentication
        [Default: (null)]
        type: str

- sftp_private_key_password
        Password for the sftp private key used for authentication
        [Default: (null)]
        type: str

- sftp_root_directory
        File browser root directory
        [Default: (null)]
        type: str

- sftp_server_alive_interval
        sftp keep alive interval
        [Default: (null)]
        type: int

- sftp_username
        Username for sftp
        [Default: (null)]
        type: str

- state
        Create or delete the connection?
        (Choices: present, absent)[Default: present]
        type: str

- username
        Username for the connection
        [Default: (null)]
        type: str

- validate_certs
        Validate ssl certs?
        [Default: True]
        type: bool


AUTHOR: Pablo Escobar Lopez (@pescobar)
        METADATA:
          status:
          - preview
          supported_by: community


EXAMPLES:

- name: Create a new rdp connection
  scicore.guacamole.guacamole_connection:
    base_url: http://localhost/guacamole
    auth_username: guacadmin
    auth_password: guacadmin
    connection_name: test_name_3
    protocol: rdp
    hostname: 192.168.33.44
    port: 3389
    username: rdp_user
    password: rdp_pass

- name: Create a new vnc connection with sftp enabled
  scicore.guacamole.guacamole_connection:
    base_url: http://localhost/guacamole
    auth_username: guacadmin
    auth_password: guacadmin
    connection_name: test_vnc
    protocol: vnc
    hostname: 192.168.33.44
    port: 5900
    username: rdp_user
    password: rdp_pass
    sftp_enable: true
    sftp_port: 22
    sftp_hostname: 192.168.11.11
    sftp_server_alive_interval: 10
    sftp_username: sftp_user
    sftp_password: adsfadfasfdasf

- name: Delete a connection
  scicore.guacamole.guacamole_connection:
    base_url: http://localhost/guacamole
    auth_username: guacadmin
    auth_password: guacadmin
    connection_name: test_CC
    state: absent


RETURN VALUES:

connection_info:
    description: Information about the created or updated connection
    type: dict
    returned: always
message:
    description: Some extra info about what the module did
    type: str
    returned: always
```

## Output of "ansible-doc scicore.guacamole.guacamole_user"

```
> GUACAMOLE_USER    (~/.ansible/collections/ansible_collections/scicore/guacamole/plugins/modules/guacamole_user.py)

        Create or delete a guacamole user

  * This module is maintained by The Ansible Community
OPTIONS (= is mandatory):

- allow_access_after
        Hour to allow access. Format --:--
        [Default: (null)]
        type: str

- allowed_connections
        List of connections where this user can connect
        [Default: (null)]
        elements: str
        type: list

= auth_password
        Guacamole admin user password to login to the API

        type: str

= auth_username
        Guacamole admin user to login to the API

        type: str

= base_url
        Url to access the guacamole API

        type: str

- disable_account_after
        Date to disable the account in format dd/mm/yyyy
        [Default: (null)]
        type: str

- disabled
        Disable the account?
        [Default: (null)]
        type: bool

- do_not_allow_access_after
        Hour to disallow access. Format --:--
        [Default: (null)]
        type: str

- email
        Email of the user
        [Default: (null)]
        type: str

- enable_account_after
        Date to enable the account in format dd/mm/yyyy
        [Default: (null)]
        type: str

- expired
        Is this account expired?
        [Default: (null)]
        type: bool

- full_name
        Full name of the user
        [Default: (null)]
        type: str

- organization
        Organization of the user
        [Default: (null)]
        type: str

- organizational_role
        Role of the user in his/her organization
        [Default: (null)]
        type: str

- password
        Password for the new user
        [Default: (null)]
        type: str

- state
        Create or delete the user?
        (Choices: present, absent)[Default: present]
        type: str

- timezone
        User timezone
        [Default: (null)]
        type: str

= username
        Name of the new user to create

        type: str

- validate_certs
        Validate ssl certs?
        [Default: True]
        type: bool


AUTHOR: Pablo Escobar Lopez (@pescobar)
        METADATA:
          status:
          - preview
          supported_by: community


EXAMPLES:

- name: Create a new guacamole user
  scicore.guacamole.guacamole_user:
    base_url: http://localhost/guacamole
    auth_username: guacadmin
    auth_password: guacadmin
    username: test_user_3
    password: user_pass
    allowed_connections:
      - connection_1
      - connection_2
    full_name: John Foo
    email: john@email.com
    organization: company_bar

- name: Delete a guacamole user
  scicore.guacamole.guacamole_user:
    base_url: http://localhost/guacamole
    validate_certs: false
    auth_username: guacadmin
    auth_password: guacadmin
    username: test_user_3
    state: absent


RETURN VALUES:

user_info:
    description: Information about the created or updated user
    type: dict
    returned: always
message:
    description: Message about what the module did
    type: str
    returned: always
```
