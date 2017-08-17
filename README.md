# XWiki Platform GWT

Retired Google WebToolkit modules.

## Debugging ##

### Debugging the Java code ###

#### Debugging the client side Java code ####

Starting with GWT 2.0 we can run and debug the client side Java code directly in the browser. This is called Development Mode. Follow these steps:

1. Copy `src/test/resources/start_wysiwyg_noserver_debug.sh` from the `xwiki-gwt-wysiwyg-server` module to the root of your XWiki Enterprise installation. For the standard jetty+hsqldb distribution this is the directory where `start_xwiki.sh` is located.
1. `chmod 755 start_wysiwyg_noserver_debug.sh`
1. Open `start_wysiwyg_noserver_debug.sh` with a text editor and make sure that the variables defined at the beginning of the file match what you have on your system.
1. The GWT Development Mode requires GWT Java sources (so not only the byte code) to be in the class path. The debug script looks for the needed dependencies in the local maven repository. Building the `xwiki-gwt-*` modules ensures these dependencies are in your local maven repository. Alternatively you can edit the debug script and reference these dependencies with a different path.
1. Start the server (e.g. `./start_xwiki.sh`).
1. `./start_wysiwyg_noserver_debug.sh`
1. Connect to the specified port using your IDE. The client side code is in the `xwiki-gwt-wysiwyg-client` maven module. Make sure you have imported it in your IDE.
1. The GWT Development Mode window should have opened. Click "Copy to Clipboard" to copy the startup URL.
1. Paste the startup URL in your browser's address bar and load it.
1. You may be asked to login since you're going to edit a wiki page in WYSIWYG mode.
1. You may be asked to install the GWT Developer plugin, if this is the first time you try to debug GWT code in Development Mode.
1. If you receive a message that tells you your GWT code needs to be recompiled just refresh the page. Note that the exiting JavaScript code of the editor (`resources/js/xwiki/wysiwyg/xwe`) might be affected so you should backup it if you don't have other means of restoring it.
1. At this point the WYSIWYG editor should be loaded and you should be able to add break points using your Java IDE. Currently the editor toolbar is badly displayed in debug mode due to some CSS issue we need to fix.

#### Debugging the server side code ####

You can debug the server side code remotely. Start your wiki in debug mode (e.g. using the ##start_xwiki_debug.sh## script) and then connect with your IDE to the specified port. The server side code of the WYSIWYG editor is in the ##xwiki-gwt-wysiwyg-server## module. Make sure you have imported it in your IDE.

### Debugging the JavaScript code ###

The JavaScript code of the WYSIWYG editor is obfuscated by default to reduce its size. To be able to debug it you have to rebuild the editor using the detailed GWT compilation style. One way to achieve this is with the `-Pdev` maven profile. Alternatively you can edit the pom of the `xwiki-gwt-wysiwyg-server` module and change the value of the `gwt.style` property. After you rebuild the editor, update your XWiki Enterprise instance and clean your browser's cache. Note that with the detailed style the JavaScript code is huge (around 5MB currently) so most of the debuggers will respond pretty slow. Sometimes you can save time by adding your break points from the JavaScript code. Edit the `<hash>.cache.html` file that is loaded by your browser and add `debugger` where you want the execution to be stopped. A quick way to find methods in the detailed JavaScript code is to search for `<ClassName>.prototype`, e.g. `RichTextArea.prototype`.
