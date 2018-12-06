Redirects process stdout to tmp file descriptor
`diff <(sort file1) <(sort file2)`

>(  ) syntax for when you want to use it as output, e.g.
`someprogram --logfile >( gzip > out.log.gz )`
