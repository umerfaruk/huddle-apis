# Introduction #

Forms provide a way for a client to interact with a server by submitting data to the server. A server uses forms to tell clients how to submit requests for further processing. Through the use of forms, an API can be self-documenting rather than relying on out-of-band information.

A form provides rich controls that clients can use to provide a more satisfying experience to human beings, i.e. through the use of client-side validation.

In the example below, a form has been constructed that can be used to submit a pizza order. In this example the form is in XML, however forms can be represented in other media types.

```
<form method="post" action="/order" enctype="application/xml">
    <input type="text" name="customer_name" required="true" />
    <input type="email" name="customer_email" required="true" />
    <input type="text" name="customer_telephone" required="true" />
    <input type="multiline" name="address" required="true" />

    <input name="pizza_size" type="enumerated" required="true">
        <option value="small" />
        <option value="medium" />
        <option value="large" />
    </input>

    <input type="enumerated" parent="pizza_size" name="pizza_base" required="true">
        <option value="deep" />
        <option value="thin" />
        <option parent="large" value="extremecheese" />
    </input>

    <input type="enumerated" name="pizza" required="true">
        <option value="meat" />
        <option value="veggie" />
        <option value="fish" />
        <option value="pineapple" />
    </input>
</form>
```

A person uses a native iPhone client to order from pizza.example.com. The client asks the server for the form above, by GETing the http://pizza.example.com/order URI. The client displays the form to the customer and allows them to make their order.

The customer must first enter their details into the form, their name, email address, telephone number and address. As the customer types into each field, the client validates the input using the metadata provided on the form. For example, the customer email input has a type of 'email'. This tells the client that the customer must enter a valid email address. All of the inputs in this form are required, therefore the client knows that it shouldn't submit the order until the customer has entered something for every input.

The customer then selects what their order is. They first select the size of pizza they want, one of small, medium or large. As the pizza\_size input has a type of 'enumerated', the client knows that the customer can only select one of the options given.

The second part of the order is what type of base the customer wants on their pizza. The pizza\_size input has been set as the parent of the pizza\_base input. Due to this relationship, the customer can only select certain pizza bases per size of pizza, for example the 'extremecheese' base is only available as a large pizza, whereas the deep and thin bases are available on any size pizza.

Finally the customer selects the type of pizza they want.

With the form fully filled in, and validated by the client, the customer submits their order. The client uses the metadata supplied on the form to submit the following request to http://pizza.example.com/order. The request is made using a HTTP POST because the method attribute of the form was set to post. The server has informed the client that it expects the form to be submitted in XML format by setting the enctype attribute to application/xml.

```
<request>
    <customer_name>Mario</customer_name>
    <customer_email>mario@mushroomkingdom.com</customer_email>
    <customer_telephone>5557776666</customer_telephone>
    <customer_address>
        101 Plumbing Avenue,
        Brooklyn,
        NY USA 34256
    </customer_address>
    <pizza_size>large</pizza_size>
    <pizza_base>thin</pizza_base>
    <pizza>meat</pizza>
</request>
```

For reference, the same form request in JSON:

```
{
    "customer_name": "Mario",
    "customer_email": "mario@mushroomkingdom.com",
    "customer_telephone": "5557776666",
    "customer_address": "101 Plumbing Avenue,\u000ABrooklyn,\u000ANY USA 34256"
    "pizza_size": "large",
    "pizza_base": "thin",
    "pizza": "meat"
}
```

At some date in the future, pizza.example.com adds a new Extreme Pizza to its menu. Any requests for the pizza.example.com/order form now include the 'extremepizza' pizza. Crucially, the ordering client does not have to be updated to let people order this pizza, all it has to do is show the new option and let it be ordered.

## Form element ##
_Attributes_

| **Name** | **Description** |
|:---------|:----------------|
|action    |URL to use for form submission|
|method    |Protocol method to use for form submission|
|enctype   |Form data set encoding type to use for form submission|

_Child elements_

| **Name** | **Description** |
|:---------|:----------------|
|input     |A typed data field|

_Media type rendering_

  * JSON: An array of objects with the name 'forms'. Each object represents a single form.
  * XML: Each form is represented by a 'form' tag-pair.

The _action_ attribute must be a valid non-empty URL potentially surrounded by spaces. If the _action_ attribute is not specified or is invalid, the form should not be submitted.

The _method_ attribute is an enumerated attribute with the following keywords and states. The state of the method is dependent on the protocol scheme of the _action_ attribute URL.

For HTTP:

  * The keyword post, represents the POST state, and indicates the HTTP POST method.

If the state of the _method_ attribute is invalid, empty, or not specified, the POST state should be assumed.

The _enctype_ attribute is an enumerated attribute with the following keywords and states:

  * The "application/xml" keyword and corresponding state.
  * The "application/json" keyword and corresponding state.

If the state of the _enctype_ attribute is invalid, empty, or the attribute is missing, the "application/xml" state should be used.

## Input element ##
_Attributes_

| **Name** | **Description** |
|:---------|:----------------|
|name      |The name of the input to use for form submission|
|type      |Type of the input|
|value     |The initial value of the input|
|required  |Whether the input value is required for form submission|
|errorType |Indicates that there was an error with the submitted value on form submission|
|parent    |Associates this input with another input|

_Child elements_

| **Name** | **Description** |
|:---------|:----------------|
|option    |The option element represents an option in an enumerated input.|

_Media type rendering_

  * JSON: An array of objects with the name 'inputs'. Each object represents a single input.
  * XML: Each input is represented by an 'input' tag-pair.

The _name_ attribute gives the name of the input, as used in form submission. Its value must not be an empty string.

The _type_ attribute controls the data type of the input element. It is an enumerated attribute.
The following table lists the keywords and states for the attribute and the expected data type for the value of the input.

The missing value default is 'text'.

| **Keyword** | **State** | **Data type** | **Description** |
|:------------|:----------|:--------------|:----------------|
|hidden       |Hidden     |An arbitrary string|The value of the hidden input must be submitted as-is during form submission. It should not be modified by the client.|
|enumerated   |Enumerated |An enumerated value|The value of an enumerated input must be one of the values specified by the option elements contained by the input.|
|text         |Text       |Text with no line breaks|An input with an editable value that can accept a string. The value must be stripped of any line breaks before form submission.|
|multiline    |Multiline  |Text with line breaks|An input with an editable value that can accept any string. Line breaks are allowed.|
|password     |Password   |Text with no line breaks. Sensitive information.|An input with an editable value that accepts a string. The value entered must be obscured if displayed. The value should not be obscured on form submission. The value must be stripped of any line breaks before form submission.|
|email        |Email      |An email       |An input with an editable value that can accept any string. The value must be stripped of any line breaks, leading and trailing whitespace before form submission.|

The value of the _Multiline_ state is editable. It can include line breaks in the form of U+000A LINE FEED (LF) characters. On form-submission, line breaks should be translated into single U+000A LINE FEED (LF) characters.

The value of the _Email_ state is editable. It can contain any text string without line breaks, however a value that is not an email is likely to be rejected by the server. The client is expected to validate the inputted value as a valid email address.

A valid email address is a value that matches the HTML specification's definition of a valid email address. http://www.whatwg.org/specs/web-apps/current-work/multipage/states-of-the-type-attribute.html#valid-e-mail-address.

The valid email address definition is provided below for practicalities sake.

A valid e-mail address is a string that matches the email production of the following ABNF, the character set for which is Unicode. This ABNF implements the extensions described in RFC 1123. [ABNF](ABNF) [RFC5322](RFC5322) [RFC1034](RFC1034) [RFC1123](RFC1123)

```
  email         = 1*( atext / "." ) "@" label *( "." label )
  label         = let-dig [ [ ldh-str ] let-dig ]  ; limited to a length of 63 characters by RFC 1034 section 3.5
  atext         = < as defined in RFC 5322 section 3.2.3 >
  let-dig       = < as defined in RFC 1034 section 3.5 >
  ldh-str       = < as defined in RFC 1034 section 3.5 >
```

The following JavaScript and Perl compatible regular expression is an implementation of the above definition.

```
/^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/
```

The value of the _enumerated_ state must be one of the values specified in the contained option elements. The value should be used exactly as specified in the value attribute of the option.

The _value_ attribute gives the initial value of the input element. If the input is being displayed to the user, the initial value should be displayed and be editable. If the input is left unchanged, the initial value should be submitted.

The _required_ attribute is a boolean attribute. Valid values (case-insensitive) are "true" or "false". A value of true requires a valid non-empty value for the input element. If the required attribute is not specified or has a value of false, the input element is considered optional. The attribute value should be considered false, if the value is missing or invalid.

The _errorType_ attribute can be used when the value submitted to the server for the input element was invalid, due to the validation rules used at the server. An errorType can be specified for any of the form inputs. The _errorType_ value is an absolute URI that identifies the error type. When dereferenced, it should provide human readable documentation for the error type. The errorType string should be used as the primary identifier for the error type.

The _parent_ attribute associates this input with another input inside the same form. Only input elements with a type of _enumerated_ can be associated with one another. An association creates a parent-child relationship between two enumerated inputs. The parent-child relationship is used to restrict the child to certain values.

The association must be one way only, i.e. there should not be any cyclical dependencies between inputs.

Below two enumerated inputs have been associated with one another. The relationship instructs the client that when the 'tea' value is selected in the 'typeofdrink' input, only the values in the 'drink' input with a parent equal to 'tea' ('oolong' and 'assam') can be submitted for the 'drink' input. Likewise when 'coffee' is selected, 'flatwhite' and 'longblack' become valid values for the 'drink' input.

```
<input type="enumerated" name="typeofdrink">
    <option value="coffee" />
    <option value="tea" />
</input>
 
<input type="enumerated" name="drink" parent="typeofdrink">
    <option value="oolong" parent="tea" />
    <option value="assam" parent="tea" />
    <option value="flatwhite" parent="coffee" />
    <option value="longblack" parent="coffee" />
</input>
```

Option elements in the child that do not specify a _parent_ attribute value are valid values for any of the parent values.

If the value of the _parent_ attribute is missing or invalid, the input element should act as-if no _parent_ attribute was specified.

If a value in the parent is selected that has no corresponding options in the child input, no value should be submitted for the child input.

## Option element ##

_Attributes_

| **Name** | **Description** |
|:---------|:----------------|
|value     |The value that this option represents|
|parent    |The parent value that enables this option|

The option element represents an option in an enumerated input.

## Submitting a form ##

When a form is submitted, the data in the form is converted into the structure specified by the _enctype_, and then sent to the _action_ destination using the given _method_.

The method for constructing and submitting the form data is based on the _method_ state and the scheme of the _action_.

| |method|
|:|:-----|
|scheme|POST  |
|http|Submit as entity body|
|https|Submit as entity body|

The behaviour of schemes not listed in the table above are not defined by this specification.

**Submit as entity body**

Let _entity body_ be the result of constructing the form data set.

Let _Content-Type_ be determined by the state of _enctype_:

  * If enctype is **application/xml**, let _Content-Type_ be application/xml
  * If enctype is **application/json**, let _Content-Type_ be application/json

Submit the _entity body_, as _Content-Type_, to _action_ using the HTTP method given by _method_.

**Constructing the form data set**

All character encoding should be UTF-8.

  1. Let _form data_ set be a list of name-value pairs, initially empty.
  1. Loop through each input element in the form being submitted:
    1. Let _name_ be the value of the input elements _name_ attribute.
    1. Append an entry to the _form data set_ with _name_ as the name, and the value of the input element as the value.

**application/xml**

  1. Let result be an empty string.
  1. Open a `<request>` element in result.
  1. For each entry in the form data set:
    1. Open an element in result with the name set to the entry's name.
    1. Append the entry's value to result.
    1. Close the element in result.
  1. Close the `<request>` element in result.

**application/json**

  1. Let result be an empty string.
  1. Using the rules of the JSON specification RFC4627.
  1. Open a new object in result.
    1. For each entry in the form data set:
    1. Add a name-value pair to the object in result with the name set to the entry's name and the value set to the entry's value. The value should be of type string.
  1. Close the object in result.
