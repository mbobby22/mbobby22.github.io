---
layout: post
title:  "Javascript function check dropdown selection"
date:   2018-04-02 20:30:00 +0200
categories: coding javascript jquery select dropdown
---

Function to check if value has already been selected in dropdown, using `jQuery` features.
Based on 'this' element and 'selector' class of all other elements.
Error uses message translated and outputted by `PHP`.

{% highlight javascript %}
/**
 * Function to check if value has already been selected in dropdown.
 * Based on 'this' element and 'selector' class of all other elements.
 * @param element
 * @param selector
 */
function selectedExists(element, selector) {
    var selected = [];
    var errorMessage = '<?php echo $this->translate('You have selected the same value. Please check and try again'); ?>';

    $(selector).each(function (index) {
        if (selected.includes(element.selectedIndex) === true) {
            $().toastmessage('showErrorToast', errorMessage);
            element.selectedIndex = 0;
            return false;
        } else {
            selected.push(this.selectedIndex);
        }
    });
}
{% endhighlight %}

Usage: `'onchange' => 'selectedExists(this, ".calledZoneId")',`