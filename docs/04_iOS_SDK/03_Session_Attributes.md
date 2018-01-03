TestFairy can collect additional information from your session, which can help you generate better insights. 

In order to set a unique identifier for your user in the current session, please see the document on [Identifying Your Users](https://docs.testfairy.com/iOS_SDK/Identifying_Your_Users.html).

### Syntax

`[TestFairy setAttribute:@"<key>" withValue:@"<value>"]`

The first value is a string `key` to help you search for the attribute in your session. The second paramter, `value`, is any string value for the attribute associated with the session. Neither value can be nil. These attributes are available later in the session recording page, is available via API, and is searchable.

### Example

```
[TestFairy setAttribute:@"name" withValue:@"John Snow"];
[TestFairy setAttribute:@"phone" withValue:@"+672-14-5109"];
[TestFairy setAttribute:@"age" withValue:@"20"];
[TestFairy setAttribute:@"favorite_color" withValue:@"blue"];
```

This will mark the session with the values above, so when you review the recording, you have more information about the person running the app.

### Notes

1. `setAttribute:` may be called many times. 
2. You may call `setAttribute` before or after `begin`.
3. You can only store a maximum of 64 keys. The keys can be a maximum of 64 characters. The values can have a maximum of 1000 characters.
