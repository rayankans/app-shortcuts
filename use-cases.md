# Shortcuts v2 Use Cases

Below is a collection of real-world uses cases provided by various organizations. They are bucketed into whether they need access to manifest-defined shortcuts (read-only or read/write) or not.

## **Do** require access to manifest-defined shortcuts

1. **Status:** Applications want to update specific shortcuts with up-to-date status information. Examples include
    * a shortcut to a particular payment method (e.g., gift card) which could be updated to include the current balance.
    * a shortcut to a user’s messaging inbox could be updated to reflect the current unread count.
2. **Localization:** Apps that do localization on the client-side want to be able to define shortcuts in the app’s default language via the manifest and then update the label given to each dynamically via the API.
3. **Behavior Change:** Applications may want one behavior from a shortcut when the PWA is not open and a different behavior when it is. For example, a messaging app might want to redirect you to the homepage after the user composes a new message via a shortcut when the app is closed, whereas they may want to close the new window if the app is already open (so as not to have two windows open to the app’s homepage). This could be accomplished by updating the URL of the shortcut to include a query string parameter while running. Note: This would require that the change to the manifest-defined shortcut would not persist beyond the app being closed, which has not been considered in other use cases.

## **Do not** require access to manifest-defined shortcuts

1. **Recent activities:** Applications want to offer quick access to recent activities. Examples include
    * recent conversations the user was involved in,
    * recent searches they conducted,
    * and recent documents they authored or accessed.

## Additional use cases for consideration

1. **Grouped shortcuts:** Applications want to bundle shortcuts into (potentially-collapsible) sub-groups. For example:
    * recently-authored and recently shared files;
    * payment methods; and
    * user accounts/email addresses/conversations with a collected unread counter.
