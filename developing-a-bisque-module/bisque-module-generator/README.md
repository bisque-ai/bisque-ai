# BisQue Module Generator

## BisQue Module Generator CLI

Standardizes and automates the process of creating modules that can be integrated in the Bisque web application. This command line interface (CLI) supports multiple inputs and outputs of type image, table or file. Output images will be displayed in the module results section in Bisque while tables and file outputs will have links to their respective resource service where they can be downloaded and visualized.

\
Custom outputs or interactive parameters will require users to manually edit some files. Regardless of your application, it is a good idea to follow this guide and use the CLI to automate a big part of the process and avoid common bugs.&#x20;

### Further Customization

Once you have reached the end of this guide, if you need more customization, please take a look at the [Official Bisque Documentation](https://ucsb-vrl.github.io/bisqueUCSB/module-development.html) for a full guide on how to build modules from scratch. The [XML section](https://ucsb-vrl.github.io/bisqueUCSB/module-xml.html) will provide you with some common features that users might want to include in their module and how to define them in the xml. The vision for this project is for users to build their modules without the need to edit the module files which can be tedious and time consuming to write. We will continuously update this guide and the code to include as many features as possible.

{% content-ref url="../further-customization.md" %}
[further-customization.md](../further-customization.md)
{% endcontent-ref %}

## Reasons to Make a BisQue Module

One of the main reasons to convert your source code into a BisQue module is for reproducibility. Researchers in the field have exceptional work, but poor coding practices hinders the widespread adoption and justification of their findings. This is by no means their fault, but luckily help is here, _BisQue is here_.

BisQue Modules are analysis extensions to the bisque system that allow users to incorporate their own custom analysis scripts written in `Python, C++, Java, MATLAB, etc.` that perform high-end computations such as deep learning based methods for use by the system and others.
