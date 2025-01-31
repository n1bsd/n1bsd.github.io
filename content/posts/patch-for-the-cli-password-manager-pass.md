+++
slug = 'patch-for-the-cli-password-manager-pass'
title = 'Patch for the CLI password manager _pass_'
date = 2016-05-25T00:00:00+00:00
draft = false
tags = ["CLI", "Software"]
+++
I use [Pass](https://www.passwordstore.org/) to store and synchronize all my passwords.

When I use Pass via SSH on a remote system in order to retrieve a password, I cannot make use of it's clipboard feature. In order to output the password without actually displaying it, I wrote the following patch which prints the password in red on a red background while still being able to be manually copied to the clipboard:

```
 src/password-store.sh | 19 +++++++++++++------
 1 file changed, 13 insertions(+), 6 deletions(-)

diff --git a/src/password-store.sh b/src/password-store.sh
index d535a74..f4e1cfe 100755
--- a/src/password-store.sh
+++ b/src/password-store.sh
@@ -223,8 +223,8 @@ cmd_usage() {
             List passwords.
         $PROGRAM find pass-names...
             List passwords that match pass-names.
-        $PROGRAM [show] [--clip,-c] pass-name
-            Show existing password and optionally put it on the clipboard.
+        $PROGRAM [show] [--clip,-c] [--hidden,-h] pass-name
+            Show existing password and optionally put it on the clipboard or hide it.
             If put on the clipboard, it will be cleared in $CLIP_TIME seconds.
         $PROGRAM grep search-string
             Search for password files containing search-string when decrypted.
@@ -294,23 +294,30 @@ cmd_init() {
 }

 cmd_show() {
-    local opts clip=0
-    opts="$($GETOPT -o c -l clip -n "$PROGRAM" -- "$@")"
+    local opts clip=0 hidden=0
+    opts="$($GETOPT -o ch -l clip,hidden -n "$PROGRAM" -- "$@")"
     local err=$?
     eval set -- "$opts"
     while true; do case $1 in
         -c|--clip) clip=1; shift ;;
+        -h|--hidden) hidden=1; shift ;;
         --) shift; break ;;
     esac done

-    [[ $err -ne 0 ]] && die "Usage: $PROGRAM $COMMAND [--clip,-c] [pass-name]"
+    [[ $err -ne 0 ]] && die "Usage: $PROGRAM $COMMAND [--clip,-c] [--hidden,-h] [pass-name]"

     local path="$1"
     local passfile="$PREFIX/$path.gpg"
     check_sneaky_paths "$path"
     if [[ -f $passfile ]]; then
         if [[ $clip -eq 0 ]]; then
-            $GPG -d "${GPG_OPTS[@]}" "$passfile" || exit $?
+            if [[ $hidden -eq 0 ]]; then
+                $GPG -d "${GPG_OPTS[@]}" "$passfile" || exit $?
+                       else
+                local pass="$($GPG -d "${GPG_OPTS[@]}" "$passfile" | head -n 1)"
+                [[ -n $pass ]] || exit 1
+                               echo -e "\e[0;31;41m$pass\e[0m"
+                       fi
         else
             local pass="$($GPG -d "${GPG_OPTS[@]}" "$passfile" | head -n 1)"
             [[ -n $pass ]] || exit 1
```
