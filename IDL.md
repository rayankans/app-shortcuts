```webidl
// Already defined in https://w3c.github.io/manifest/#dom-shortcutitem
dictionary ShortcutItem {
  required USVString name;
  USVString short_name;
  USVString description;
  required USVString url;
  sequence<ImageResource> icons;
};

// New definitions
[Exposed=Window]
partial interface Navigator {
  [SecureContext, SameObject] readonly attribute ShortcutsManager shortcuts;
};

[Exposed=(Window,SecureContext)]
interface ShortcutsManager {
    Promise<sequence<ShortcutItem>> getAll();
    Promise<void> set(sequence<ShortcutItem> shortcutItems);
};
```