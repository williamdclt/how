`ssh -N -L 1111:{rds-host}:5432 {login}@{bastion-server} -i {path/to/key}`
eg: `ssh -N -L 1111:legacy-staging.cupg32dk1xbq.eu-west-2.rds.amazonaws.com:5432 ec2-user@ec2-35-178-105-1.eu-west-2.compute.amazonaws.com -i devops/eb.pem`

Update docker-compose:
`DATABASE_URL: "postgres://{username}:{password}@host.docker.internal:1111/{dbname}"`

