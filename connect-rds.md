`ssh -N -L 1111:{rds-host}:5432 {bastion-server} -i {path/to/key}`

Update docker-compose:
`DATABASE_URL: "postgres://legacy:4Xbdw0Bn6ZygJhs6@host.docker.internal:1111/legacy"`

