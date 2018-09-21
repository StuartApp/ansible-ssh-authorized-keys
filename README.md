# Stuart SSH authorized keys role

## Usage example:

```
ssh_authorized_keys_users:
  username1:
    - ssh-rsa key1 comment1
    - ssh-rsa key2 comment2
  username2:
    - ssh-rsa key3 comment3
    - ssh-rsa key4 comment4
```

**WARNING**: *more than 1 level of nested groups won't work*

```
ssh_authorized_keys_groups:
  groupname1:
    - username1
    - username2
  groupname2:
    - username3
    - groupname1
```

```
ssh_authorized_keys:
  <username>:
    path: <path of the authotized_keys file> # optional
    manage_dir: <true|false> # optional, defaults to ssh_authorized_keys_manage_dir
    exclusive: <true|false> # optional, defaults to ssh_authorized_keys_exclusive
    auth_keys:
      - groupname1
      - username3
```

There is also `ssh_authorized_keys_default`. This has the same structure as `ssh_authorized_keys`, and both are always `deepmerge`d

## `deepmerge` filter

This role also adds a new filter named `deepmerge` which does what `combine` does but it also
merges lists inside dictionaries. Example:

```
dict1:
  a:
    ab:
      - 1
      - 2

dict2:
  a:
    ab:
      - 3
  b:
    ba:
      - 1
```

When using combine like this `dict1 | combine(dict2)` we get:

```
a:
  ab:
    - 3
b:
  ba:
    - 1
```

So the list in `ab` is overwritten. `deepmerge` *fixes* this. Running this `dict1 | deepmerge(dict2)` we get:

```
a:
  ab:
    - 1
    - 2
    - 3
b:
  ba:
    - 1
```

Now `ab` has a list with all elements of both lists appended.

`deepmerge` filter can be use anywhere **if** the *ssh_authorized_keys* role is loaded. For variables it is evaluated after the role is included, for other roles, these have to be called *after* *ssh_authorized_keys*.
