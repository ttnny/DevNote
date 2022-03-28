# Remap Home & End Keys on External Keyboard

In **Terminal** app:

```
$ cd ~/Library
$ mkdir KeyBindings
$ nano KeyBindings/DefaultKeyBinding.dict
```

Copy & paste these into the `DefaultKeyBinding.dict`:

```
{
  "\UF729"  = moveToBeginningOfParagraph:;										// home
  "\UF72B"  = moveToEndOfParagraph:; 													// end
  "$\UF729" = moveToBeginningOfParagraphAndModifySelection:;	// shift-home
  "$\UF72B" = moveToEndOfParagraphAndModifySelection:; 				// shift-end
  "^\UF729" = moveToBeginningOfDocument:;											// ctrl-home
  "^\UF72B" = moveToEndOfDocument:; 													// ctrl-end
  "^$\UF729" = moveToBeginningOfDocumentAndModifySelection:;	// ctrl-shift-home
	"^$\UF72B" = moveToEndOfDocumentAndModifySelection:;				// ctrl-shift-end
}
```

May need to restart to take effect.



---

https://www.maketecheasier.com/fix-home-end-button-for-external-keyboard-mac/