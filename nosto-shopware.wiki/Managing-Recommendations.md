The plugin adds a default set of recommendation elements to the shop, that can be moved/removed as you see fit.

Recommendations are by default added to the following pages:

* The home page
* The category pages
* The product pages
* The search result page
* The shopping cart page

**Note:** All recommendation elements are by default appended to the Shopware `frontend_index_content` block in the theme. 

## Moving recommendations

In order to move one of the default recommendation blocks, you need to override the template file that outputs them. Shopware supports overriding templates by copying the original file and placing it in your theme folder, keeping the complete file structure the identical to what it is in the plugin.

Example:

Moving one of the recommendation elements on the category pages to the top of the page, while keeping the other recommendation at the bottom.

The file you need to override is `frontend/plugins/nosto_tagging/listing/index.tpl`. Copy this file and place it in `SHOPWARE/themes/Frontend/THEME_NAME/frontend/plugins/nosto_tagging/listing/index.php`. Then edit the file to move the recommendations as below.

    {**
    * Copyright (c) 2015, Nosto Solutions Ltd
    * All rights reserved.
    *
    * Redistribution and use in source and binary forms, with or without
    * modification, are permitted provided that the following conditions are met:
    *
    * 1. Redistributions of source code must retain the above copyright notice,
    * this list of conditions and the following disclaimer.
    *
    * 2. Redistributions in binary form must reproduce the above copyright notice,
    * this list of conditions and the following disclaimer in the documentation
    * and/or other materials provided with the distribution.
    *
    * 3. Neither the name of the copyright holder nor the names of its contributors
    * may be used to endorse or promote products derived from this software without
    * specific prior written permission.
    *
    * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
    * AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
    * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
    * ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
    * LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
    * CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
    * SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
    * INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
    * CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
    * ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
    * POSSIBILITY OF SUCH DAMAGE.
    *
    * @author Nosto Solutions Ltd <shopware@nosto.com>
    * @copyright Copyright (c) 2015 Nosto Solutions Ltd (http://www.nosto.com)
    * @license http://opensource.org/licenses/BSD-3-Clause BSD 3-Clause
    *}
    
    {block name="frontend_listing_index_listing" prepend}
    	<div class="nosto_element" id="nosto-page-category1"></div>
    {/block}

    {block name="frontend_index_content" append}
    	<div class="nosto_element" id="nosto-page-category2"></div>
    	{include file="frontend/plugins/nosto_tagging/listing/category_tagging.tpl"}
    {/block}

You need to double check that you have both the `frontend_listing_index_listing` and `frontend_index_content` blocks in your theme, and that they are positioned accordingly. If they are not, or you wish to add the recommendations to an other block, you can do so freely.

**Note:** The inclusion of the category tagging, i.e. `{include file="frontend/plugins/nosto_tagging/listing/category_tagging.tpl"}` is required. This file needs to be included, but it does not matter where on the page it is placed.

## Removing recommendations

If you wish to remove one of the default recommendation elements you can easily do so by going to the plugin configuration page in the backend, and for the correct account toggle of the visibility of the recommendation. These settings are found on the Recommendations page of the account management.

Recommendations that are not visible will not be populated with content by Nosto. The recommendation element placeholder will still be added to the site, but it will remain empty and hidden to the users.

## Adding new recommendations

You can add your own customised recommendation elements to your shop by adding the recommendation placeholder elements in your theme. The placeholders are simple html div elements, e.g. `<div class="nosto_element" id="my-custom-recommendation-id"></div>`.

Note that the element IDs need to match defined element's in Nosto. These you need to create and configure for you're Nosto account by logging in to the [Nosto backend](https://my.nosto.com/).