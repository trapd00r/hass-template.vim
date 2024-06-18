# hass-template.vim
Write homeassistant templates in vim and render it using hass-cli


### Grab all the states from homeassistant

... remove the `update` states and wrap them in `{{states()}}` jinja2 template

Now you can select any (or all) states and render it using `gq`

```bash
hass-cli state list | awk '{print $1}' | sort -u | grep -vP '^update|update$' | perl -pe 's#^(.+)$#"{{states(🤩$1🤩)}}"#' | perl -pe 's/🤩/"/g' | vim -c setf\ jinja
```

Bonus:

Grab all entities and their values

```bash
 hass-cli state list | awk '{print $1}' | sort -u | grep -vP '^update|update$' | perl -pe 's/^(.+)$/$1:$1/' | while IFS=: read -r entity value; do printf "%s %s\n" $entity "{{states('$value')}}" | hass-cli template -  ; done 
 ```

[![asciicast](https://asciinema.org/a/565704.svg)](https://asciinema.org/a/565704)
