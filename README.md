# DISCONTINUED

There's no value in maintaining this project. I got lazy and opted for a simple patch.

If you also want to use it, here it is:

Use it with 
- npm: https://www.npmjs.com/package/patch-package
- pnpm: `pnpm patch`

```patch
diff --git a/lib/cue.d.ts b/lib/cue.d.ts
index b0060f1a4dc3a6944e29517b03b03a39d85df42f..7da5f99ef0eb827fbae0c33c5cf3842b902ef361 100644
--- a/lib/cue.d.ts
+++ b/lib/cue.d.ts
@@ -1,7 +1,8 @@
-import { ICueSheet } from "./types";
+import { ICueSheet } from './types';
+
 /**
  * Parse function
- * @param filename Filename path to cue-sheet to be parsed
+ * @param contents the cue-sheet contents to be parsed
  * @return CUE-sheet information object
  */
-export declare function parse(filename: string): ICueSheet;
+export declare function parse(contents: string): ICueSheet;
diff --git a/lib/cue.js b/lib/cue.js
index 0661c750e72ef6060531b0360ad4179655027bd4..cc068e524840a67545e930002dc4a9c9a1dcc5bf 100644
--- a/lib/cue.js
+++ b/lib/cue.js
@@ -28,30 +28,16 @@ const commandMap = {
  * @param filename Filename path to cue-sheet to be parsed
  * @return CUE-sheet information object
  */
-function parse(filename) {
+function parse(contents) {
     const cuesheet = new cuesheet_1.CueSheet();
-    if (!filename) {
-        console.log('no file name specified for parse');
-        return;
-    }
-    if (!fs.existsSync(filename)) {
-        throw new Error('file ' + filename + ' does not exist');
-    }
-    cuesheet.encoding = chardet.detect(fs.readFileSync(filename));
-    let encoding = 'utf8';
-    if (cuesheet.encoding.startsWith('ISO-8859-')) {
-        encoding = 'binary';
-    }
-    else if (cuesheet.encoding.toUpperCase() === 'UTF-16 LE') {
-        encoding = 'utf16le';
-    }
-    const lines = fs.readFileSync(filename, { encoding, flag: 'r' })
-        .replace(/\r\n/, '\n').split('\n');
+    const lines = contents.replace(/\r\n/, '\n').split('\n');
     lines.forEach(line => {
-        if (!line.match(/^\s*$/)) {
-            const lineParser = command_1.parseCommand(line);
-            commandMap[lineParser.command](lineParser.params, cuesheet);
+      if (!line.match(/^\s*$/)) {
+        const lineParser = command_1.parseCommand(line);
+        if (commandMap[lineParser.command]) {
+          commandMap[lineParser.command](lineParser.params, cuesheet);
         }
+      }
     });
     if (!cuesheet.files[cuesheet.files.length - 1].name) {
         cuesheet.files.pop();

```
