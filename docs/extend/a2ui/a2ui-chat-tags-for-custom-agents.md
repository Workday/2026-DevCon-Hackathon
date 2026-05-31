# **Reference: Chat Tags for Custom Agents**

| Container Tags | Editable Tags | Read-Only Tags | Invisible Tags |
| :---- | :---- | :---- | :---- |
| [chatCard](#chatcard) | [chatCheckBox](#chatcheckbox) | [chatButton](#chatbutton) | [chatHidden](#chathidden) |
| [chatColumn](#chatcolumn) | [chatChoicePicker](#chatchoicepicker) | [chatDivider](#chatdivider) | [chatLoop](#chatloop) |
| [chatRow](#chatrow) | [chatDateTimeInput](#chatdatetimeinput) | [chatIcon](#chaticon) |  |
|  | [chatTextField](#chattextfield) | [chatImage](#chatimage) |  |
|  |  | [chatText](#chattext) |  |

# Container Tags

## chatCard 

Type: ChatCardTag extends ChatContainerTag (Container tag)

A card container that holds an arbitrary list of chat tags. Children will be displayed as if they were in a chatColumn.

|Property|Type|Description|
| --- | --- | --- |
|children|List<ChatTag>|(Required) The list of tags that will be displayed in the container.|

## chatColumn

Type: ChatColumnTag extends ChatLayoutTag (Container tag)

Displays children vertically, as if they were inside a column.

|Property|Type|Description|
| --- | --- | --- |
|children|List<ChatTag>|(Required) The list of tags that will be displayed in the container.|
|align|Align|Defines the alignment of children along the cross axis (horizontally).<p>Possible values:<ul><li>START<li>CENTER<li>END<li>STRETCH (Default)<li>BASELINE|
|justify|Justify|Defines the arrangement of children along the main axis (vertically). Use `spaceBetween` to push items to the edges (e.g. header at top, footer at bottom), or start/end/center to pack them together.<p>Possible values<ul><li>START (Default)<li>CENTER<li>END<li>SPACE_BETWEEN<li>SPACE_AROUND|

## chatRow

Type: ChatRowTag extends ChatLayoutTag (Container tag)

Displays children horizontally, as if they were inside a row.

|Property|Type|Description|
| --- | --- | --- |
|children|List<ChatTag>|(Required) The list of tags that will be displayed in the container.|
|align|Align|Cross-axis alignment of children. (Default: STRETCH)<p>Possible values:<ul><li>START<li>CENTER<li>END<li>STRETCH<li>BASELINE|
|justify|Justify|Main-axis distribution of children. (Default: START)<p>Possible values:<ul><li>START<li>CENTER<li>END<li>SPACE_BETWEEN<li>SPACE_AROUND|

# Editable Tags

Editable tags enable users to interact with and change the data on the page that’s rendered on the chat window. Editable tags use these required attributes:

* `valuePath`: Defines the data that will be passed back to an agent during a submission. The valuePath field is always required for editable tags.<p>The `valuePath` acts as the name of the variable in the data section. Its a JSON path, so something like "/foo/bar/barbar" will create a nested structure that will be rendered as follows:
```
"foo": {
  "bar": {
    "barbar": ""
  }
}
```
* `value`: Defines the value to be placed in the data section. The `value` field is required but can be an empty string if no data is available for that field.

## chatCheckBox

Type: ChatCheckBoxTag (Leaf tag)

A boolean checkbox. Edit-only (not rendered on view pages).

|Property|Type|Description|
| --- | --- | --- |
|value|BooleanScript|(Required) Value assigned upon rendering the page; can be changed by the user on edit pages.|
|valuePath|String|(Required) The model attribute to which this field is associated.|
|label|DynamicBinding<String>|Label displayed for the checkbox.|

Example:
```
{
  "type": "chatCheckBox",
  "value": "<% getEmail.subscribe %>",
  "label": "Subscribe to Newsletter",
  "valuePath": "/subscribe"
}
```

## chatChoicePicker

Type: ChatChoicePickerTag (Leaf tag)

A dropdown/picker supporting single or multi-select. Allows selecting one or more options from a list.

|Property|Type|Description|
| --- | --- | --- |
|value|ListScript|(Required) The list of values of the selected options.|
|valuePath|String|(Required) The path to the value of the selected option in the model.|
|displayStyle|DisplayStyle|Visual display style of the picker.<p>Possible values:<ul><li>CHECKBOX<li>CHIPS|
|filterable|boolean|Whether the choice picker is filterable by user input.|
|label|DynamicBinding<String>|Label displayed for the choice picker.|
|options|ListScript|Defines the list of choices in the picker. Options is expected to be a List of Maps, with each Map containing entries with two keys, one for label and one for value.|
|variant|ChoicePickerVariant|Defines the picker as either supporting the selection of only one option or multiple options.<p>Possible values:<ul><li>MULTIPLE_SELECTION<li>MUTUALLY_EXCLUSIVE|

Example:

```
{
  "type": "chatChoicePicker",
  "label": "Choose One",
  "valuePath": "/singleSelection",
  "value": "<% choicePickerData.singleSelection %>",
  "options": "<%
    [
      {
        'label': 'Option A',
        'value': 'a'
      },
      {
        'label': 'Option B',
        'value': 'b'
      },
      {
        'label': 'Option C',
        'value': 'c'
      }
    ]
  %>",
  "variant": "MUTUALLY_EXCLUSIVE"
}
```

## chatDateTimeInput

Type: ChatDateTimeInputTag (Leaf tag)

A date and/or time picker bound to the data model. Allows a user to select a date and/or a time. The selected date and/or time value in ISO 8601 format. If not yet set, initialize with an empty string.

|Property|Type|Description|
| --- | --- | --- |
|value|StringScript|(Required) The current date/time value in ISO format. Typically bound to a data path.|
|valuePath|String|(Required) Path to the data in the model.|
|enableDate|boolean|Shows the date picker. This attribute cannot be set to false, so a date is always selectable by the user.|
|enableTime|boolean|If true, shows the time picker, allowing the user to select a time.|
|label|DynamicBinding<String>|Label displayed for the date/time input.|
|max|String|Maximum allowed date/time value (ISO 8601 format).<p>Invalid date values are not selectable. The max attribute only works when enableTime is false and enableDate is true.|
|min|String|Minimum allowed date/time value (ISO 8601 format).<p>Invalid date values are not selectable. The min attribute only works when enableTime is false and enableDate is true.|

Example:

```
{
  "type": "chatDateTimeInput",
  "label": "Date and Time",
  "value": "",
  "valuePath": "/dateAndTime",
  "enableDate": true,
  "enableTime": true
}
```

## chatTextField

Type: ChatTextFieldTag (Leaf tag)

A text input field for chat pages.

|Property|Type|Description|
| --- | --- | --- |
|value|StringScript|(Required) Initial value of the text field.|
|valuePath|String|(Required) The path to the data in the model.|
|label|DynamicBinding<String>|Label displayed for the text field.|
|validationRegexp|String|Regular expression used to validate the text field's value. Not supported by Workday Modulr A2UI Renderer.|
|variant|TextFieldVariant|Defines the type of input field to display.<p>Possible values:<ul><li>SHORT_TEXT (Default)<li>LONG_TEXT<li>NUMBER<li>OBSCURED|

Example:

```
{
  "type": "chatTextField",
  "value": "<% getEmail.name %>",
  "label": "Name",
  "valuePath": "/name"
}
```

# Read-Only Tags

## chatButton

Type: ChatButtonTag (Leaf tag)

A button that fires an action event sent to the agent when clicked. The agent will decide what to do based on the action.

|Property|Type|Description|
| --- | --- | --- |
|action|String|(Required) The action/event string identifying which button was pressed. Defines the name of the action that will be sent to the agent when this button is clicked.|
|label|DynamicBinding<String>|(Required) The text to display on the button.|
|submitDataModel|boolean|If true, the context of this button's action will be populated with all of the data in the data model. If false, the event will have an empty context.|
|variant|ChatButtonVariant|Rendering hint for the button style.<p>Possible values:<ul><li>DEFAULT<li>PRIMARY<li>BORDERLESS|

Action example:
```
{
  "version": "v0.9",
  "action": {
    "name": "confirm",
    "surfaceId": "booking",
    "context": {
      "details": {
        "datetime": "2025-12-16T19:00:00Z",
        "guests": "3"
      }
    }
  }
}
```

`chatButton` example:
```
{
  "type": "chatButton",
  "label": "Click Me!",
  "action": "clicked",
  "variant": "PRIMARY"
}
```

## chatDivider

Type: ChatDividerTag (Leaf tag)

A horizontal divider line. Only supported for use in chatColumn tags.

|Property|Type|Description|
| --- | --- | --- |
|axis|Axis|Orientation of the divider line. Possible values:

* HORIZONTAL (Default)|

Example:
```
{
   "type": "chatDivider"
}
```

## chatIcon

Type: ChatIconTag (Leaf tag)

Renders a named icon. The name field defines the icon to be displayed. Not all icons are currently supported.

|Property|Type|Description|
| --- | --- | --- |
|name|IconName|(Required) Name of the icon to render.<p>Possible values:<ul><li>ACCOUNT_CIRCLE<li>ADD<li>ARROW_BACK<li>ARROW_FORWARD<li>ATTACH_FILE<li>CALENDAR_TODAY<li>CALL<li>CAMERA<li>CHECK<li>CLOSE<li>DELETE<li>DOWNLOAD<li>EDIT<li>EVENT<li>ERROR<li>FAST_FORWARD<li>FAVORITE<li>FAVORITE_OFF<li>FOLDER<li>HELP<li>HOME<li>INFO<li>LOCATION_ON<li>LOCK<li>LOCK_OPEN<li>MAIL<li>MENU<li>MORE_VERT<li>MORE_HORIZ<li>NOTIFICATIONS_OFF<li>NOTIFICATIONS<li>PAUSE<li>PAYMENT<li>PERSON<li>PHONE<li>PHOTO<li>PLAY<li>PRINT<li>REFRESH<li>REWIND<li>SEARCH<li>SEND<li>SETTINGS<li>SHARE<li>SHOPPING_CART<li>SKIP_NEXT<li>SKIP_PREVIOUS<li>STAR<li>STAR_HALF<li>STAR_OFF<li>STOP<li>UPLOAD<li>VISIBILITY<li>VISIBILITY_OFF<li>VOLUME_DOWN<li>VOLUME_MUTE<li>VOLUME_OFF<li>VOLUME_UP<li>WARNING|

Example:
```
{
  "type": "chatIcon",
  "name": "ARROW_BACK"
}
```

## chatImage

Type: ChatImageTag (Leaf tag)

An image for chat pages. Support for the fit and variant fields is currently limited. In some environments these fields have no impact; in others they change the size and shape of the image.

|Property|Type|Description|
| --- | --- | --- |
|url|StringScript|(Required) Location of the image.|
|fit|ChatImageFit|A hint for how the image should be scaled within its container.<p>Possible values:<ul><li>CONTAIN<li>COVER<li>FILL<li>NONE<li>SCALE_DOWN|
|variant|ChatImageVariant|A hint for rendering the image size/shape.<p>Possible values:<ul><li>ICON<li>AVATAR<li>SMALL_FEATURE<li>MEDIUM_FEATURE<li>LARGE_FEATURE<li>HEADER|

Example:
```
{
  "type": "chatImage",
  "url": "https://picsum.photos/200/150",
  "fit": "CONTAIN",
  "variant": "SMALL_FEATURE"
}
```

## chatText

Type: ChatTextTag (Leaf tag)

Displays a styled text string. Although chatText is read-only, you can populate the valuePath attribute value. While a user might not be able to change the value, the text field's value can be sent as part of the context when a button is pushed.

|Property|Type|Description|
| --- | --- | --- |
|text|StringScript|(Required) The text to display.|
|valuePath|String|The path to the data in the model.|
|variant|TextVariant|Rendering hint for the text style.<p>Possible values:<ul><li>H1<li>H2<li>H3<li>H4<li>H5<li>CAPTION<li>BODY|

Example:
```
{
  "type": "chatText",
  "text": "<% product.price %>",
  "variant": "H2",
  "valuePath": "/price"
}
```

# Invisible Tags

## chatHidden

Type: ChatHiddenTag (Leaf tag)

A non-rendered field that binds a value silently to the model. Works like a regular editable tag. Used for including data in a button's action when that data isn't actually shown in the UI.

|Property|Type|Description|
| --- | --- | --- |
|value|ObjectScript|(Required) The value to be assigned to the hidden field.|
|valuePath|String|(Required) The model attribute to which this field is associated.|

Example:
```
{
  "type": "chatHidden",
  "value": "<% product.productId %>",
  "valuePath": "/productId"
}
```

## chatLoop

Type: ChatLoopTag (Container tag)

Iterates over a list datasource and renders child tags once per item. Works like a regular PMD loop tag but for use on A2UI pages.

|Property|Type|Description|
| --- | --- | --- |
|as|VariableDefinition|(Required) Variable name referring to the current element of on being processed.|
|children|List<ChatTag>|(Required) The list of chat tags to render for each element in the datasource.|
|on|ListScript|(Required) Datasource that the template will iterate over.|