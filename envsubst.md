# envsubst

```sh
$ cat << EOF > hello.txt
hello $USER
EOF

$ cat hello.txt
hello $USER

$ USER=deptno envsubst < hello.txt
hello deptno

$ cat hello.txt | USER=deptno envsubst
hello deptno
```

# link
- [[terminal]]
- [[kubectl]]
