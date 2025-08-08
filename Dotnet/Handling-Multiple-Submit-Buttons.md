# Four Ways of Handling Multiple Submit Buttons in ASP.NET Core

## Multiple buttons with different names

In this technique you use form tag helper of ASP.NET Core and submit the form to an action method. The relevant markup from the Index view that does this is shown below:

```html
<form asp-controller="Home" asp-action="ProcessForm" method="post">    
    <input type="submit" name="save" value="Save" />
    <input type="submit" name="cancel" value="Cancel" />
</form>
```

There are two submit buttons - one with name attribute set to save and the other with name of cancel. When you click a particular button, its name and value is sent to the server (just like any other input element). The ProcessForm() action needs to grab these values to detect which one was clicked. 

This is how the Index view looks like in the browser:

```cs
[HttpPost]
public IActionResult ProcessForm(string save, string cancel)
{
    if (!string.IsNullOrEmpty(save))
    {
        // save button
    }
    if (!string.IsNullOrEmpty(cancel))
    {
        // cancel button
    }
    return View();
}
```

## Multiple buttons with the same name

This technique is similar to the previous technique. But the buttons involved are given the same name in the HTML markup.

```html
<form asp-controller="Home" asp-action="ProcessForm" method="post">
    <input type="submit" name="submit" value="Save" />
    <input type="submit" name="submit" value="Cancel" />
</form>
```

Notice that the name attribute of both the buttons is set to submit and their value attribute is set to some string. Then the ProcessForm() action accepts a single parameter - submit - that receives the value of the button clicked by the user. This is shown below:

```cs
[HttpPost]
public ActionResult ProcessForm(string submit)
{
    switch(submit)
    {
        case "Save":
            // save
            break;
        case "Cancel":
            // cancel
            break;
    }
    return View();
}
```

As you can see the submit parameter is checked for its value. 

## HTML5 formaction and formmethod attributes

In this technique you use the formaction and formmethod attributes introduced in HTML5. These attributes can be set on the buttons under consideration. The formaction attribute indicates the form's action whereas the formmethod attribute indicates the form submission method.
The modified form markup is shown below:

```html
<form>
    <input type="submit" name="save" value="Save" formaction="/Home/SaveForm" formmethod="post" />
    <input type="submit" name="cancel" value="Cancel" formaction="/Home/CancelForm" formmethod="post" />
</form>
```

As you can see the formaction attribute of the save button submits to SaveForm() action whereas the cancel button submits to the CancelForm() action. The formmethod attribute is set to post for both the buttons. Also notice that the form tag helper no longer has asp-controller and asp-action attributes. The SaveForm() and CancelForm() actions are shown below:

```cs
[HttpPost]
public ActionResult SaveForm()
{
    return View();
}

[HttpPost]
public ActionResult CancelForm()
{
    return View();
}
```

## jQuery / JavaScript code

If above techniques doesn't meet your requirements you can always use jQuery or JavaScript to programmatically set the form action. The following markup shows how the modified form looks like:

```html
<form method="post">
    <input type="submit" id="save" value="Save" />
    <input type="submit" id="cancel" value="Cancel" />
</form>
```

This time the ID attribute of the buttons is set to save and cancel respectively. This way you can easily access them in the jQuery code. The jQuery code that wires the click event handlers for these buttons and sets the form action dynamically is shown below:

```js
$(document).ready(function () {
    $("#save").click(function () {
        $("form").attr("action", "/Home/SaveForm");
    });
    $("#cancel").click(function () {
        $("form").attr("action", "/Home/CancelForm");
    });
});
```

The click event handler of the save button sets the action attribute of the form to `/Home/SaveForm`. The click event handler of the cancel button sets the action to `/Home/CancelForm`.

# References

* https://www.binaryintellect.net/articles/2678a2f2-3236-45a6-a0e5-e6340d9930d5.aspx