=======
iocage
=======

Install iocage and install FreeBSD jails with it.

.. note::

    See the full `Salt Formulas installation and usage instructions
    <http://docs.saltstack.com/en/latest/topics/development/conventions/formulas.html>`_.

Available modules
=================

.. contents::
    :local:

``iocage``
---------

Manage freebsd jails:

- fetch a release
- create a jail
- start / stop / restart / destroy a jail
- list jails / release / templates
- list / get / set jail properties


# Usage
The main keys are name, properties, jail_type and template_id.

```
iocage_test_jail:
  iocage.managed:
    - name: test_jail
    - properties:
      - ip4_addr:lo1|10.1.1.20/32
    # optional default is full
    - jail_type: full
    # required only if jail_type is template-clone
    - template_id: ""
```

### name

The name property is the tag that is used to create the field. This is a required field.

### properties

A list of key value pairs. All properties listed in `iocage defaults` can be set here. Also pkglist key can be set here.

### jail_type

The jail_type of jail. Can be one of full , clone (-c), base (-b), template-clone, empty (-e). To create a template jail, just set 'template=yes' in properties.

### template_id:

If the jail jail_type is template clone, then the template to be cloned is defined here. This is required if the jail_type is set to template_clone


# Examples

Create a full jail with default properties:
```
iocage_test_jail:
  iocage.managed:
    - name: test_jail
```

Create a full jail with some properties set:
```
iocage_test_jail:
  iocage.managed:
    - name: test_jail
    - properties:
      - ip4_addr:lo1|10.1.1.20/32
```

Create a cloned jail:
```
iocage_test_jail:
  iocage.managed:
    - name: test_jail
    - jail_type: clone
    - properties:
      - ip4_addr:lo1|10.1.1.20/32
```

Create a template jail:
```
iocage_test_jail:
  iocage.managed:
    - name: test_jail
    - properties:
      - ip4_addr:lo1|10.1.1.20/32
      # note the quotes for yes. Without it yaml parses it as true
      - template = "yes"
```

Clone a template. This will clone the template with tag test_template
```
iocage_test_jail:
  iocage.managed:
    - name: test_jail
    jail_type: template-clone
    template-id: test_template
    - properties:
      - ip4_addr:lo1|10.1.1.20/32
```
