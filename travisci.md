# TravisCI cheatsheet

#### Using awscli v2 on Travis CI

```yaml
install:
  - if ! [ - x"$(command -v aws)" ]; then curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" ; unzip awscliv2.zip ; sudo ./aws/install ; fi
```

