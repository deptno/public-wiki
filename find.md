# find

## rename
```sh
find . -name '*.ts' -not -path 'node_modules/*' -not -path 'dist/*' -not -path '*/__tests__/*' -exec bash -c 'mv "$0" "${0%.ts}.mts"' {} \;
```

## link
- [[terminal]]
- [[fd]]
