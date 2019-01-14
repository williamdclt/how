You have:
```
o---o---o---o---o  master
         \
          o---o---o---o---o  next
                           \
                            o---o---o  topic
```

You want:
```                                                                  
o---o---o---o---o  master
        |        \
        |         o'--o'--o'  topic
         \
          o---o---o---o---o  next
```

Do:
`git rebase --onto master topic next`
