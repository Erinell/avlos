Avlos Configuration
*******************

Device Spec
-----------

A Device Spec is a YAML file that outlines the attributes and functionalities of a device, serving as a blueprint for code generation with Avlos. It is structured as a hierarchical tree, with each node representing a specific feature or function of the device.

Structure of a Device Spec
^^^^^^^^^^^^^^^^^^^^^^^^^^

The top level of the spec typically starts with the `name` of the device. This is followed by `remote_attributes`, a list of attributes that describe various aspects of the device. Each attribute in this list can itself be a complex entity, often consisting of several key-value pairs:

- `name`: The name of the attribute.
- `dtype`: The data type of the attribute (e.g., `uint32`, `float`, `string`, `bool`).
- `unit` (optional): The unit of measurement, applicable for numerical data types (e.g., `volt`, `ampere`, `meter/s`).
- `meta` (optional): Metadata for the attribute, which can include flags like `dynamic` or `export`.
- `getter_name` / `setter_name`: The names of the getter and setter functions associated with the attribute.
- `summary` (optional): A brief description of the attribute.
- `flags` (optional): A list of specific flags relevant to the attribute (e.g., error types). This is useful to define bit fields that can represent specific conditions in the system.
- `options` (optional): Alternative to flags, a list of exclusive options like an enum. This is useful to enums that represent different system states.


Sub-attributes and Nested Structures
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Attributes can have nested `remote_attributes`, creating a tree-like structure. This is particularly useful for grouping related functionalities or aspects of the device, such as `scheduler`, `controller`, `motor`, etc. Each of these groupings can have its own set of attributes, mirroring the structure described above.

Functions
^^^^^^^^^

Besides standard attributes that behave like getters/setters, the spec can define functions for the device, such as `save_config`, `erase_config`, `reset`, etc. Functions can have a list of arguments, and can return a value. Functions are typically defined with:

- `name`: The name of the function.
- `summary` (optional): A brief description of the function.
- `caller_name`: The function prototype to call for this function endpoint.
- `dtype`: The return data type, often `void` for function.
- `arguments` (optional): A list of arguments for functions, each with its own `name` and `dtype`.
- `unit` (optional): The unit of measurement, applicable for numerical data types (e.g., `volt`, `ampere`, `meter/s`).
- `meta`: Metadata for the function, similar to attributes.

Implementing Your Own Device Spec
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To implement your own device spec for use with Avlos:

1. Define the device name at the top of the YAML file.
2. Enumerate the attributes (`remote_attributes`) of your device, following the structure described above.
3. For each attribute, specify its `name`, `dtype`, `getter_name`, `setter_name` (if applicable), `summary`, and any other relevant details.
4. Group related attributes under nested `remote_attributes` to create a clear, hierarchical structure.
5. Add any special actions or commands your device supports, with appropriate details.

This structured approach allows Avlos to understand the capabilities and interface of your device, enabling efficient and accurate code generation.


Output Config
-------------

The output config defines the output modules that will be used and their options. Example, showing C code generation for embedded devices:

.. code-block::
    
    generators:
        generator_c:
            enabled: true
            paths:
                output_header: outputs/header.h
                output_impl: outputs/header.c
            header_includes:
            - src/header.h
            impl_includes:
            - src/test.h

CLI Usage
---------

Using a Local File
^^^^^^^^^^^^^^^^^^

Ensure the device spec and the output config exist in the current folder.

.. code-block:: console

    avlos from file device.yaml

This will generate the outputs according to the configuration in the output config file.


Using a URL
^^^^^^^^^^^

Ensure the output config exists in the current folder.

.. code-block:: console

    avlos from url https://your.url/spec.yaml

This will generate the outputs according to the configuration in the output config file.