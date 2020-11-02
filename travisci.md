# TravisCI cheatsheet

#### Using awscli v2 on Travis CI

- source: [original](https://travis-ci.community/t/support-for-awscli-v2-in-ci-images/7197), [archive](https://archive.is/eZrQW)

```yaml
install:
  - if ! [ - x"$(command -v aws)" ]; then curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" ; unzip awscliv2.zip ; sudo ./aws/install ; fi
```

