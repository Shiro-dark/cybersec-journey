#!/bin/bash
echo " ghost uuid scan and clean"
echo "--------------------------"

EXT_DIRS=$(find ~/.mozilla/firefox -type d -name 'moz-extension+++*')

for EXT_PATH in $EXT_DIRS; do
  UUID=$(basename "$EXT_PATH" | sed 's/moz-extension+++//')

  # check if uuid exists in any pref.js
  FOUND=$(grep -r "$UUID" ~/.mozilla/firefox/*/prefs.js)

  if [[ -z "$FOUND" ]]; then
    echo "→ unlinked uuid found : $UUID"
    echo "→ path: $EXT_PATH"
    echo "→ removing.."
    rm -rf "$EXT_PATH"
  else
    echo "→ linked uuid: $UUID → legit"
  fi
done

echo "scan complete"

![script 1](screenshots/uuid-script.jpg)
