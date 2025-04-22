# How to capture Custom Data on Direct Uploader

## Overview

Often, you would like to capture additional data from your audience via the Direct Uploader widget for your events or competitions. This guide will show you how to setup custom inputs on your Direct Uploader widget. First thing first, navigate to the Direct Uploader management screen - Aggregate > Direct Uploader, select your favourite Direct Uploader widget. When you are on your Direct Uploader widget, click "Configure" tab, and choose a template where you would like to add your own custom input fields.

## Adding Custom fields

Direct Uploader accepts input fields that are prefixed with "custom-data-" and followed by the name of the input field. Note: The name of the input field must contain alphanumeric or underscore characters **only**. e.g `custom-data-**gender**` is correct, `custom-data-**your-gender**` is incorrect. See following sample code, which captures Company, Gender and About from the audience

```

<div class="row">
    <div class="col-md-12">
        <div class="st-form-group">
            <label>Company</label>
            <input type="text" class="st-form-control" name="custom-data-company" value="" placeholder="Company" />
        </div>
    </div>
</div>
<div class="row">
    <div class="col-md-12">
        <div class="st-form-group">
            <label>Gender</label>
            <select name="custom-data-gender" class="st-form-control">
                <option></option>
                <option value="male">Male</option>
                <option value="female">Female</option>
            </select>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-md-12">
        <div class="st-form-group">
            <label>Tell us more about yourself</label>
            <textarea name="custom-data-more_about_yourself" class="st-form-control"></textarea>
        </div>
    </div>
</div>
    
```

Save your settings to see additional fields appear on your Direct Uploader widget or click on the preview.

\</br/> When content come through from your Direct Uploader widget, your custom input will be captured in the `__external_data` column on the Nosto's UGC **Tile** object.

```
// Tile Object
{
    ...
    "\_\_external\_data": {
        "\_id": { "$id": "123" }
        "firstname": "John",
        "lastname": "Smith",
        "email": "John.Smith@stackla.com",
        "ip": "0.0.0.0",
        "terms\_and\_conditions\_url": "http://your-t-and-c.com",
        "terms\_and\_conditions": 1,
        "company": "Nosto",
        "gender": "Male",
        "more\_about\_yourself": "I love Visual UGC"
    },
    ...
}
```

#### Custom validation

Now that you have added your custom input fields, It's time to add some validation to those input fields. By default, your custom input will be validated against our server by the following rules:

* Field name must not exceed 1024 characters
* Input value must not exceed 1024 characters
* Input must not contain HTML elements
* Message field supports up to 32000 characters
* Custom\_input field supports up to 250 characters

However, You can also provide your own validation by adding a callback in the Custom JS tab

```

        window.Stackla.GoConnectCallbacks = {
            onBeforeSave: function (formData, errorMessagesFromServer) {

                if (typeof(formData['custom-data-company']) !== 'undefined' && !formData['custom-data-company']) {

                    errorMessagesFromServer['custom-data-company'] = 'Hey, please enter a company!';

                    return false;

                } else {

                    return true;

                }

            }
        };
    
```

Note the callback **onBeforeSave** will be passed with 2 parameters when the beforeSave event is fired.

* **formData** is the data to be sent to Nosto's UGC for validation before save happens.
* **errorMessagesFromServer** is an error object Nosto's UGC server returns when data is validated by Nosto's UGC. If you would like to add your own validation logic, simply adding for modifying this object.
* Remember to **return true** or **false** from your callback to indicate whether or not to go ahead with saving the data to Nosto's UGC

#### Returning Custom Data from Nosto's UGC

In Nosto'sual UGC, we take security seriously, custom inputs from your Direct Uploader widget will be **hidden** from all Nosto's UGC public endpoints. However, there are some situations where you would like certain input to be exposed to the public, for example, you are capturing "company" from your Direct Uploader widget, and you want "company" data to be displayed on your widget. You can achieve this by adding **data-permission="public"** to your "company" input on the Direct Uploader widget prior to your Direct Uploader widget go live.

When you adding **data-permission="public"** data attribute to your custom inputs, you indicate to the Direct Uploader widget that these fields will be exposed to the public. Visual UGC will capture your public data in the **external\_data** column on your Visual UGC **Tile** object.

```
// Tile Object
{
    ...,
    "external_data": {
        "company": "Visual UGC"
    }
    ...,
}

// Custom Widget JS - Display "company" data on your Widget
Callbacks.prototype.onCompleteJsonToHtml = function (tileObject, t) {
    if (t && t.external_data && t.external_data.company) {
        tileObject.append(t.external_data.company);
    }
    return tileObject;
};
```

_Note that you can find your public data in the **external\_data** column on the Visual UGC Tile Object, external\_data is a subset of the **\_\_external\_data** column. **\_\_external\_data** on the other hand is private and it is hidden from all public endpoints by default, you can only access it on Moderation View or from Visual UGC API_
