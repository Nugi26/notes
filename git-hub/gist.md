## gist
gh gist --public create <filename(s)...> -d <message>

## create from stdin
gh gist create -

## create from command output
cat file.txt | gh gist create -f <new filename>

## open created gist in browser
gh gist -w