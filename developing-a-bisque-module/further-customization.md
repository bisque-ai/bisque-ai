# Further Customization

## Module XML

{% hint style="info" %}
**NOTE**

FILENAME: `NAME_OF_MODULE.xml`, where `NAME_OF_MODULE` is replaced by the name of your module, i.e. `Dream3D.xml`.
{% endhint %}

The module definition file lays out the interface that the system can call the module with. The simplest form simply lists the name, location and arguments needed to run the modules. Here is an example of a module definition document:

***

#### **Example** XML File



{% code title="MetaData.xml" %}
```xml
<?xml version="1.0" encoding="utf-8"?>
<module name="MetaData" type="runtime">

    <!-- Comments are OK -->
    <tag name="inputs">
        <tag name="image_url" type="resource">
            <template>
                <tag name="accepted_type" value="image" />
                <tag name="accepted_type" value="dataset" />
                <tag name="label" value="Image to extract metadata" />
                <tag name="prohibit_upload" value="true" type="boolean" />
            </template>
        </tag>

        <tag name="mex_url"  type="system-input" />
        <tag name="bisque_token"  type="system-input" />
    </tag>

    <tag name="outputs">
         <tag name="metadata" type="tag">
            <template>
                <tag name="label" value="Extracted metadata" />
            </template>
         </tag>
    </tag>

    <tag name="execute_options">
        <tag name="iterable" value="image_url" type="dataset" />
        <!-- Example for a blocked iteration -->
        <tag name="blocked_iter" value="true" />
    </tag>

    <tag name="module_options" >
        <tag name="version" value="3" />
    </tag>

    <tag name="display_options" >
       <tag name="group" value="Examples" />
    </tag>

    <tag name="help" type="file" value="public/help.html" />
    <tag name="thumbnail" type="file" value="public/thumbnail.png" />

    <tag name="title" type="string" value="MetaData" />
    <tag name="authors" type="string" value="The Bisque team" />
    <tag name="description" type="string" value="This module annotates an image with its embedded metadata." />
</module>
```
{% endcode %}

The definition above allows the BisQue system to call the module by creating a MEX.

The module definition document is actually a templated MEX document. The template parameters in this case are used to render the UI for this module if the user does not want to fully implement the UI. More about this will follow.

A module can define inputs and outputs and rely on the automated interface generation or can provide a fully customized user interface delivered by the module server by proxying the data made available by the engine service. Input configurations may also be used by the modules that define their own interfaces since they can call renderers provided by the module service.

***

## Module Description&#x20;

Each module has to be described in various ways to be useful. Each module has a number of required, as well as optional parameters it has/may contain. Type in this case can control where the data is coming from, for example default "string" suggests the data is in-place. "file" directs the engine server to look for the file starting in the module root directory.

### **Title**

```xml
<tag name="title" value="MetaData" />  
```

### **Description**

```xml
<tag name="description" value="This module annotates an image with its embedded metadata." />   
```

### **Authors**

```xml
<tag name="authors" value="The Bisque team" /> 
```

### **Thumbnail**

```xml
<tag name="thumbnail" type="file" value="public/thumbnail.png" />   
```

### **Help**

An HTML document with a module help that the user can be directed to the document can be inline.

```xml
<tag name="help" type="file" value="public/help.html" /> 
```

### **Module Options**:

```xml
<tag name="module_options" >
    <tag name="version" value="2" />
</tag>
```

## Configurations for Images, Datasets, and Resources&#x20;

### **Label**

Specifies the label rendered before asking for a resource.

```xml
<tag name="label" value="Select input image" />
```

### **Accepted Type**

Defines multiple allowed types of input resource.

```xml
<tag name="accepted_type" value="dataset" />
<tag name="accepted_type" value="image" />
```

### **Prohibit Upload**

Used with resources of type image or dataset to specify that the uploader should not be allowed.

```xml
<tag name="prohibit_upload" value="true" type="boolean" />
```

### **Example Query**

Allow a button "from Example" specifies the query string.

```xml
<tag name="example_query" value="*GFAP*" />
```

### **Allow Blank**

Makes resource optional.

```xml
<tag name="allow_blank" value="true" type="boolean" />
```

***

## **`Image` Specific Configurations**&#x20;

### **Require Geometry**

Enforce input _image_ geometry.

Here the `z` or `t` value may be:

* null or undefined - means it should not be enforced
* 'single' - only one plane is allowed
* 'stack' - only stack is allowed

```xml
<tag name="require_geometry">
    <tag name="z" value="stack" />
    <tag name="t" value="single" />
    <tag name="fail_message" value="Only supports 3D images!" />
</tag>
```

### **Fail Message**

Specify a message to show if failed requires validation.

```xml
<tag name="fail_message" value="Only supports 3D images!" />
```

***

## **`gobject` Specific Configurations**&#x20;

### **GObject Example**&#x20;

{% code title="YOUR-MODULE.xml" %}
```xml
<gobject name="tips">
  <template>
    <tag name="gobject" value="point" />
    <tag name="require_gobjects">              
        <tag name="amount" value="many" />
        <tag name="fail_message" value="Requires selection of root tips" />
    </tag>              
  </template>
</gobject>
```
{% endcode %}

### **gobject**

Defines multiple allowed types of input gobject.

```xml
<tag name="gobject" value="point" />
<tag name="gobject" value="polygon" />
```

Semantic types can also be specified here:

```xml
<tag name="gobject" value="foreground">
    <tag name="color" value="#00FF00" type="color" />
</tag>
<tag name="gobject" value="background">
    <tag name="color" value="#FF0000" type="color" />
</tag>
```

Moreover, colors could be proposed for these semantic types to differentiate graphical annotations in modules visually.

Users can also force only semantic annotations to be created basically prohibiting creation of primitive graphical elements without semantic meaning:

```xml
<tag name="semantic_types" value="require" />
```

### **Require gobjects**

Validate input _gobject_.

```xml
<tag name="require_gobjects">
    <tag name="amount" value="many" />
    <tag name="fail_message" value="Requires select of root tips" />
</tag>
```

Configuration for `require_gobjects` consists of:

*   **amount** - constraint on the amount of objects of allowed type. The amount can take the following values:

    * null or undefined - means it should not be enforced
    * 'single' - only one object is allowed
    * 'many' - only more than one object allowed
    * 'oneornone' - only one or none
    * number - exact number of objects allowed
    * $$\leq X$$ - operand followed by a number, accepts:$$<,>,<=,>=,==$$ &#x20;



    **Example**. Note that $$<$$ sign should be encoded in an XML attribute.

    ```xml
    <tag name="amount" value="3" type="number" />
    or
    <tag name="amount" value=">=2" />
    or
    <tag name="amount" value="&lt;30" />
    ```

### **Fail Message**

Specify a message to show if failed requires validation.

```xml
<tag name="fail_message" value="Requires select of root tips" />
```

### **Color**

Specify a default color for created gobjects

```xml
<tag name="color" value="#00FFFF" type="color" />
```

**Example** with semantic types and other configuration:

```xml
<gobject name="stroke">
    <template>
        <tag name="gobject" value="freehand_line" />
        <tag name="gobject" value="foreground">
            <tag name="color" value="#00FF00" type="color" />
        </tag>
        <tag name="gobject" value="background">
            <tag name="color" value="#FF0000" type="color" />
        </tag>
        <tag name="semantic_types" value="require" />
        
        <tag name="require_gobjects">
            <tag name="amount" value=">=2" />
            <tag name="fail_message" value="Requires two polylines; 
            first one inside object of interest (foreground)
            and second across background." />
        </tag>
    </template>
</gobject>
```

***

## **`string` Specific Configurations**&#x20;

### **Label**

Specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Units**

Provides the string and possibly defines conversions, not all types support units, for example boolean does not:

```xml
<tag name="units" value="microns" />
```

### **Fail Message**

Message that will be displayed if failed the check:

```xml
<tag name="fail_message" value="We need a time series image" />
```

### **Minimum Length**

Minimum length of the required string:

```xml
<tag name="minLength" value="10" type="number" />
```

### **Maximum Length**

Maximum length of the required string:

```xml
<tag name="maxLength" value="100" type="number" />
```

### **Allow Blank**

Used to allow empty strings, true by default:

```xml
<tag name="allowBlank" value="true" type="boolean" />
```

### **Regex**

Regular expression used to validate input string:

```xml
<tag name="regex" value="[\w]" />
```

### **Default Value**

Default value of this field:

```xml
<tag name="defaultValue" value="" />
```

### **Editable**

Whether this field is editable by the user, true by default:

```xml
<tag name="editable" value="true" type="boolean" />
```

***

## **`number` Specific Configurations**&#x20;

A number can select one or more values, in case of selecting multiple values they will be selected using a multi-slider.

### **Label**

Simply specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Units**

Provides the string and possibly defines conversions, not all types support units, for example boolean does not:

```xml
<tag name="units" value="microns" />
```

### **Fail Message**

Message that will be displayed if failed the check:

```xml
<tag name="fail_message" value="We need a time series image" />
```

### **Minimum Value**

Lowest allowed value:

```xml
<tag name="minValue" value="10" type="number" />
```

### **Maximum Value**

Highest allowed value:

```xml
<tag name="maxValue" value="100" type="number" />
```

### **Allow Decimals**

Allow to acquire floating point numbers:

```xml
<tag name="allowDecimals" value="true" type="boolean" />
```

### **Decimal Precision**

How many digits after the dot are allowed:

```xml
<tag name="decimalPrecision" value="4" type="number" />
```

### **Default Value**

Default value of this field:

```xml
<tag name="defaultValue" value="" />
```

### **Editable**

Whether this field is editable by the user, true by default:

```xml
<tag name="editable" value="true" type="boolean" />
```

### **Step**

Step to be used by increment/decrement buttons:

```xml
<tag name="step" value="1" type="number" />
```

### **Show Slider**

Allows hiding the slider in case of single value picking:

```xml
<tag name="showSlider" value="false" type="boolean" />
```

### **Hide Number Picker**

Allows hiding the number selection box if only slider is preferred:

```xml
<tag name="hideNumberPicker" value="true" type="boolean" />
```

### **Multiple Values**

Using multiple values:

```xml
<tag name="myrange" type="number" >
    <value>5.6</value>
    <value>12</value>
    <value>14</value>                        
</tag>
```

***

## **`combo` Specific Configurations**&#x20;

### **Label**

Simply specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Units**

Provides the string and possibly defines conversions, not all types support units, for example boolean does not:

```xml
<tag name="units" value="microns" />
```

### **Select**

Specifies a list of select elements, provide as many as you need, this might have to be implemented as a list of values?:

```xml
<tag name="select" value="Alaska" />
<tag name="select" value="California" />
```

### **Passed Values**

Specifies a list of values passed for the corresponding select elements, this might have to be implemented as a list of values?:

```xml
<tag name="select" value="AL" />
<tag name="select" value="CA" />
```

### **Failed Value**

If the combo's value is same as fail\_value, combo's input is considered invalid and the fail\_message is displayed:

```xml
<tag name="fail_value" value="Select a choice..." />
```

### **Fail Message**

Error message that will be displayed if combo's value is null or same as fail\_value:

```xml
<tag name="fail_message" value="User needs to select a choice!" />
```

### **Editable**

Allows a combo box string to be edited directly and would allow input of values not existent in the select list:

```xml
<tag name="editable" value="true" type="boolean" />
```

***

## **`boolean` Specific Configurations**&#x20;

### **Label**&#x20;

Simply specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Fail Message**

Message that will be displayed if failed the check:

```xml
<tag name="fail_message" value="We need a time series image" />
```

### **Default Value**

Default value of this field:

```xml
<tag name="defaultValue" value="" type="boolean" />
```

### **Editable**

Whether this field is editable by the user, true by default:

```xml
<tag name="editable" value="true" type="boolean" />
```

***

## **`date` Specific Configurations**&#x20;

This renderer allow you to pick both date and time.

> **NOTE.** _Value_ contains a string in ISO standard, ex: **YYYY:MM:DDThh:mm:ss**

### **No Date**

Can hide the date picker:

```xml
<tag name="nodate" value="true" type="boolean" />
```

### **No Time**

Can hide the time picker:

```xml
<tag name="notime" value="true" type="boolean" />
```

### **Label**

Simply specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Fail Message**

Message that will be displayed if failed the check:

```xml
<tag name="fail_message" value="We need a time series image" />
```

### **Format**

Date format used by this field, default value is : YYYY:MM:DDThh:mm:ss

```xml
<tag name="format" value="YYYY:MM:DDThh:mm:ss" />
```

### **Editable**

Whether this field is editable by the user, true by default:

```xml
<tag name="editable" value="true" type="boolean" />
```

***

## **`hyperlink` Specific Configurations**&#x20;

### **Default Value**

Default value of this field:

```xml
<tag name="defaultValue" value="" />
```

### **Editable**

Whether this field is editable by the user, true by default:

```xml
<tag name="editable" value="true" type="boolean" />
```

***

## **`email` Specific Configurations**&#x20;

### **Default Value**

Default value of this field:

```xml
<tag name="defaultValue" value="" />
```

### **Editable**

Whether this field is editable by the user, true by default:

```xml
<tag name="editable" value="true" type="boolean" />
```

***

## **`bisqueresource` Specific Configurations**&#x20;

### **Resource Type**

Type of bisque resource to be selected, image by default:

```xml
<tag name="resourceType" value="image" />
```

### **Default Value**

Default value of this field:

```xml
<tag name="defaultValue" value="" />
```

### **Editable**

Whether this field is editable by the user, true by default:

```xml
<tag name="editable" value="true" type="boolean" />
```

***

## **`annotation_status` Specific Configurations**&#x20;

This element allows marking resources with annotation status as:

* **STARTED**
* **FINISHED**
* **APPROVED**

***

## **`image_channel` Specific Configurations**&#x20;

### **Example of Image Channel**

```xml
<tag name="nuclear_channel" value="1" type="image_channel">
    <template>
        <tag name="label" value="Nuclear channel" />
        <tag name="reference" value="resource_url" />
        <tag name="guess" value="nuc|Nuc|dapi|DAPI|405|dna|DNA|Cy3" />
        <tag name="fail_message" value="You need to select image channel" />
        <tag name="allowNone" value="false" type="boolean" />
        <tag name="description" value="Select an image channel representing nulcei" />
    </template>
</tag>
```

### **Label**

Simply specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Fail Message**

Message that will be displayed if failed the check:

```xml
<tag name="fail_message" value="You need to select a channel" />
```

### **Reference**

Name of the input resource that should be used to initialize this selector:

```xml
<tag name="reference" value="resource_url"/>
```

### **Guess**

Regular expression used to guess which channel should be selected by default:

```xml
<tag name="guess" value="nuc|Nuc|dapi|DAPI|405|dna|DNA|Cy3"/>
```

### **Allow None**

Allows selection of 'None' channel, used for optional channel selection:

```xml
<tag name="allowNone" value="true" type="boolean" />
```

***

## **`pixel_resolution` Specific Configurations**&#x20;

This element must have for values that represent X, Y, Z and T resolution values in microns, microns, microns and seconds respectively.

### **Example**

```xml
<tag name="pixel_resolution" type="pixel_resolution">
    <value>0</value>
    <value>0</value>
    <value>0</value>             
    <value>0</value>               
    <template>
        <tag name="label" value="Voxel resolution" />
        <tag name="reference" value="resource_url" />
        <tag name="units" value="microns" />                
        <tag name="description" value="This is a default voxel resolution and is only used during the dataset run if the image does not have one." />   
    </template>
</tag>
```

### **Label**

Simply specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Fail Message**

Message that will be displayed if failed the check:

```xml
<tag name="fail_message" value="You need to select a channel" />
```

### **Reference**

Name of the input resource that should be used to initialize this selector:

```xml
<tag name="reference" value="resource_url"/>
```

### **Units**

Provides the string and possibly defines conversions, not all types support units, for example boolean does not:

```xml
<tag name="units" value="microns" />
```

### **Description**

Used to show tool tip information:

```xml
<tag name="description" value="This is a default voxel resolution and is only used during the dataset run if the image does not have one." />
```

***

## **`annotation_attr` Specific Configurations**&#x20;

This element allows select attributes of annotations (tags/gobjects) from either whole database or constrained by a dataset. For example it can be used to select a type out of a list of all types of graphical annotations.

### **Example**

```xml
<tag name="gob_types" value="" type="annotation_attr"> 
    <tag name="template" type="template"> 
 	    <tag name="label" value="Graphical types" /> 
            <tag name="allowBlank" value="false" type="boolean" /> 
 		 
 	    <tag name="reference_dataset" value="dataset_url" /> 
 	    <tag name="reference_type" value="annotation_type" /> 
 	    <tag name="reference_attribute" value="annotation_attribute" /> 
 	
 	    <tag name="element" value="gobject" /> 
 	    <tag name="attribute" value="type" /> 
 	    <tag name="dataset" value="/data_service/" /> 
     </tag>
</tag> 
```

### **Label**

Simply specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Reference Dataset**

Name of the input resource that can be used to initialize this selector's constrained query:

```xml
<tag name="reference_dataset" value="resource_url"/>
```

### **Reference Type**

Name of the type selector that can be used to initialize this selector's constrained query:

```xml
<tag name="reference_type" value="type_combo"/>
```

### **Reference Attribute**

Name of the attribute selector that can be used to initialize this selector's constrained query:

```xml
<tag name="reference_attribute" value="attrib_combo"/>
```

### **Element**

Default value for the element to constraine query:

```xml
<tag name="element" value="gobject" /> 
```

### **Attribute**

Default value for the attribute to constrain query:

```xml
<tag name="attribute" value="type" />  
```

### **Dataset**

Default value for the dataset to constrain query:

```xml
<tag name="dataset" value="/data_service/" /> 
```

***

## **`mex` Specific Configurations**&#x20;

This element selects a previously run module execution in order to chain modules.

### **Example**

```xml
<tag name="mex_url" type="mex">
    <template>
        <tag name="label" value="Select input MEX" />
        <tag name="query" value="&amp;name=NuclearDetector3D" /> 
    </template>
</tag>    
```

### **Label**

Simply specifies the label rendered before asking for a resource:

```xml
<tag name="label" value="Select input image" />
```

### **Description**

Provides larger description shown in the tool-tip:

```xml
<tag name="description" value="This variable is very important..." />
```

### **Query**

A query string that would narrow MEX search:

```xml
<tag name="query" value="&amp;name=NuclearDetector3D" /> 
```

### **Query Selected Resource**

This option allows MEX selector to pretend it is an image resource selector by finding the name of the required resource in the selected MEX and emitting selection signal. This can be used for MEX selectors that would like to init image resolution or image channel pickers based on an input MEX:

```xml
<tag name="query_selected_resource" value="resource_url" /> 
```

***

## Data-Parallel Execution

It is possible to execute any module in a data-parallel way by passing a dataset instead of an individual image. In order to do this you need to:

1. Indicate the resource that can be iterated on
2. Allow that resource to accept datasets and possibly
3. Configure renderers for iterated run

```xml
<!-- allow iterable resource to accept datasets -->
<tag name="inputs">
    <tag name="image_url" type="resource">
        <template>
            <tag name="accepted_type" value="image" />
            <tag name="accepted_type" value="dataset" />
            ...
        </template>
    </tag> 
    ...
</tag> 

...
<!-- configure renderers for iterated run -->
<tag name="outputs">
    ...         
    <!-- Iterated outputs -->
    <tag name="mex_url" type="mex" />
    <tag name="resource_url" type="dataset" />
</tag>

.....

<!-- indicate the resource that can be iterated -->
<tag name="execute_options">
    <tag name="iterable" value="image_url" type="dataset" />
</tag> 
```

This definition is used by the module UI to add a dataset selector for this image and let module server know by sending a proper MEX that this resource should be iterated upon. Module server will create a parallel execution iterating over the selected dataset and creating an output MEX with sub MEXes for each individual image.

### **Other Data-Parallel Types**

One can also request parallel execution over resource types other than `dataset`. For example a very useful would be to request iteration over a MEX, where a module could accept parallelized MEX as input and iterate over sub-MEXs for parallelized processing of results. In order to do that we need to indicate the type of the input resource and additionally provide an xpath expression within that resource to find elements we would like to iterate over:

```xml
<tag name="execute_options">
    <tag name="iterable" value="input_mex" type="mex" >
        <tag name="xpath" value="./mex/@uri" />
    </tag>
</tag> 
```
