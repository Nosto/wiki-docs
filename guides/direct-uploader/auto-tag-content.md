# How to automatically tag data on Direct Uploader

## Overview

Often, you would like to automatically tag your content from your audience via the Direct Uploader widget for your events or competitions. This guide will show you how to setup a custom tag field on your Direct Uploader widget. First thing first, navigate to the Direct Uploader management screen - Aggregate > Direct Uploader, select your favourite Direct Uploader widget. When you are on your Direct Uploader widget, click "Configure" tab, and choose a template where you would like to add your own custom input fields.

## Adding Custom fields

Specify a field with the name custom-data-tags. The value of this field will be used to tag your content. It should match a tag name (case-insensitive) in your stack .

```

<div class="row">
    <div class="col-md-12">
        <div class="st-form-group">
            <label>Animal breed</label>
            <select name="custom-data-tags" class="st-form-control">
                 <option value="" disabled selected>Select your animal breed</option>
                 <option value="Domestic Cat">Domestic Cat</option>
                 <option value="Poodle">Poodle</option>
                 <option value="Husky">Husky</option>
            </select>     
            
        </div>
    </div>
</div>
```

Save your settings to see if the field appear on your Direct Uploader widget or click on the preview.

#### Want to stay up to date with all the latest Stackla developer news? [Sign up!](https://github.com/Stackla/docs/blob/master/docs/signup/README.md)
