# beatrizuezu.github.io

I used Jekyll and the theme is [Indigo theme](https://github.com/sergiokopplin/indigo)





### How to install

#### install RVM
```shell
\curl -sSL https://get.rvm.io | bash -s stable
```

#### Add in bash file
```shell
source ~/.profile
```

#### Install Ruby Version (2.5.1) on RVM
```shell
rvm install 2.7.2
```

#### Install Bundler and install dependencies
```
gem install bundler
bundle install
```

### How to run
```shell
bundle exec jekyll serve --config _config.yml,_config-dev.yml
```

### How to navegate
* `http://127.0.0.1:4000`
* `http://127.0.0.1:4000/admin`
