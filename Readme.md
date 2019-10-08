## List of bitmoji images:

* [here](https://api.bitmoji.com/content/templates?app_name=bitmoji&platform=ios)

Examples:

![example1](https://render.bitstrips.com/v2/cpanel/9188364-331204332_8-s5-v1.png?transparent=1&palette=1&width=200)
![example2](https://render.bitstrips.com/v2/cpanel/10200827-99065246550_1-s5-331204332_8-s5-v1.png?transparent=1&palette=1&width=200)
![example3](https://render.bitstrips.com/render/10220563/331204332_8-s5-v1.png?outfit=971785&head_rotation=1&body_rotation=7&cropped=%22body%22&width=200)

* [friends](https://hardikapatel29.github.io/bitmoji-1/harshil_hardika.html)
* [individual](https://hardikapatel29.github.io/bitmoji-1/imoji.html)
* [female outfits](https://joshcheek.github.io/bitmoji/female_outfits.html)
* [male outfits](https://joshcheek.github.io/bitmoji/male_outfits.html)
* [packs](https://joshcheek.github.io/bitmoji/packs.html)
* [find users](https://joshcheek.github.io/bitmoji/find-users.html)

I've discovered that there are additional comics, which aren't listed in the API output, eg: `10211795`

![unicorn vs dragon](https://render.bitstrips.com/v2/cpanel/10211795-99065246550_1-s5-331204332_8-s5-v1.png?transparent=1&palette=1&width=400)

## Making sense of the data

* Individual comics are in .imoji (1117 of them) replace the `%s` with an avatar id
* Multi-avatar comics are in .friends (296 of them) replace both `%s` with an avatar id (eg from .imoji or .friends)
* `https://render.bitstrips.com/v2/cpanel/:comic_id-%s-v1.png?option1&option2&...`
  * `transparent=1` to set the bg to true
  * `palette=1` no idea
  * `width=200` scale image width to 200 pixels
* `https://render.bitstrips.com/render/:comic_id/%s-v1.png?option1&option2&...`
  * `cropped=%22body%22` you can also set cropped to `"head"`
  * `outfit=971786` put the user in
  * `head_rotation=1` rotate the head to position 1
  * `body_rotation=1` rotate the body to position 1
  * `pd2={"mouth":"_blank"}` inside `pd2`, you can put any of these keys: `[  "beard",  "brow_L",  "brow_R",  "cranium",  "detail_E_L",  "detail_E_R",  "detail_L",  "detail_R",  "detail_T",  "ear_L",  "ear_R",  "eye_L",  "eye_R",  "eyelash_L",  "eyelash_R",  "eyelid_L",  "eyelid_R",  "eyelines_L",  "eyelines_R",  "forehead",  "glasses",  "hair_back",  "hair_front",  "hairbottom",  "hat",  "jaw",  "mouth",  "nose",  "pupil_L",  "pupil_R",  "stachin",  "stachout",  "tongue"]`


## Lists

Friends, generated with:

```
cat bitmoji.json | jq '.friends|map([.src]+.tags)[]' -c | ruby -ne 'puts $_ % [%w[280531978_4-s1 270452369_2-s1 121464857_22-s1]].*(2).map(&:sample)' | ruby -rjson -ne 'url, *tags = JSON.parse($_); puts "<div class=\"friends\"><img src=\"#{url}&width=300\" /><ul>#{tags.map {|t| "<li>#{t}</li>"}.join}</ul></div>"; BEGIN { puts "<style> body * { margin: 0; padding: 0; display: inline-block; } li { display: block; } .friends { width: 300px; } </style>" }'  > friends.html
```

Individual, generated with:

```
cat bitmoji.json | jq '.imoji|map([.src]+.tags)[]' -c | ruby -ne 'puts $_.sub "%s", %w[280531978_4-s1 270452369_2-s1 121464857_22-s1].sample'  | ruby -rjson -ne 'url, *tags = JSON.parse($_); puts "<div class=\"friends\"><img src=\"#{url}&width=300\" /><ul>#{tags.map {|t| "<li>#{t}</li>"}.join}</ul></div>"; BEGIN { puts "<style> body * { margin: 0; padding: 0; display: inline-block; } li { display: block; } .friends { width: 300px; } </style>" }'  > imoji.html
```

Packs (also individual), generated with:

```
cat bitmoji.json | jq '.packs[]|.templates[]|[.src]+.tags' -c | ruby -ne 'puts $_.sub "%s", %w[280531978_4-s1 270452369_2-s1 121464857_22-s1].sample'  | ruby -rjson -ne 'url, *tags = JSON.parse($_); puts "<div class=\"friends\"><img src=\"#{url}&width=300\" /><ul>#{tags.map {|t| "<li>#{t}</li>"}.join}</ul></div>"; BEGIN { puts "<style> body * { margin: 0; padding: 0; display: inline-block; } li { display: block; } .friends { width: 300px; } </style>" }'  > packs.html
```

Male outfits:

```sh
cat bitmoji.json | jq '.outfits|[.male][]|.brands[]|.outfits[]|[.image, .description]' -c | ruby -ne 'puts $_.sub /(?<=\/)\d+_\d+-s\d(?=-v)/, %w[280531978_4-s1 270452369_2-s1 128256895_1-s1].sample' |ruby -rjson -ne 'url, *tags = JSON.parse($_); puts "<div class=\"friends\"><img src=\"#{url}&width=300\" /><ul>#{tags.map {|t| "<li>#{t}</li>"}.join}</ul></div>"; BEGIN { puts "<style> body * { margin: 0; padding: 0; display: inline-block; } li { display: block; } .friends { width: 300px; } </style>" }'  > male_outfits.html
```

Female outfits:

```sh
cat bitmoji.json | jq '.outfits|[.female][]|.brands[]|.outfits[]|[.image, .description]' -c | ruby -ne 'puts $_.sub /(?<=\/)\d+_\d+-s\d(?=-v)/, %w[121464857_22-s1].sample' |ruby -rjson -ne 'url, *tags = JSON.parse($_); puts "<div class=\"friends\"><img src=\"#{url}&width=300\" /><ul>#{tags.map {|t| "<li>#{t}</li>"}.join}</ul></div>"; BEGIN { puts "<style> body * { margin: 0; padding: 0; display: inline-block; } li { display: block; } .friends { width: 300px; } </style>" }'  > female_outfits.html
```
