+++
title = 'Odoo Owl Widget'
date = 2024-07-09T10:13:55+07:00
draft = false
showtoc = false
hideSummary = true
disableShare = true
+++

In this section, I am going to create new widget base on existing widget.
Odoo use widget to display certain fields. All available widget is listed under web module.
More specifically inside `addons/web/static/src/views/fields`.

## Inheriting Existing Widget

In this example, I am going to inherit properties fields and remove `Add a property`.
Let's call this widget `new_properties`.
As usual, all modifiation will be done inside new odoo module. 

1. Create new folder `/static/src/components/new_properties`.
This folder will contains all `.js` and `.xml` that's used by new widget.  

2. Create new template view inside `./new_properties/new_properties_field.xml`.  
``` xml
<?xml version="1.0" encoding="UTF-8">
<templates xml:space="preserve">
    <t t-name="PropertiesField" t-inherit="web.PropertiesField">
        <xpath expr="//div[contains(@t-attf-class, 'o_field_property_add')]" position="replace">
            <div>
            </div>
        </xpath>
    </t>
</templates>

```

3. Create new js module inside `./new_properties/new_properties_field.js`.  

```Javascript

/** @odoo-module **/  // -> This indicate it's an odoo module. REQUIRED FOR ODOO MODULE.
import { registry } from '@web/core/registry'; //-> Use registry to register new fields
import { PropertiesField } from '@web/views/fields/properties/properties_field'; //-> Fields that's we inherite

export class NicelabelPropertiesField extends PropertiesField { //-> In here, we inherit the fields
    setup(){
        super.setup();
    }
}

NicelabelPropertiesField.template = "<module name>.PropertiesField"; // -> Change the template to custom one
NicelabelPropertiesField.supportedTypes = ["properties"]; // -> What data type this widget supported

// Register new widget as field 
registry.category("fields").add("new_properties", NicelabelPropertiesField);

```

4. Register assets inside `__manifest__.py`
``` python
    "assets": {
        "web.assets_backend": [
            "<module name>/static/src/components/*/*.js",
            "<module name>/static/src/components/*/*.xml",
        ],
    },
```

## Create new owl widget

## Repository
